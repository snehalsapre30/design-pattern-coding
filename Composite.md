# design-pattern-coding
 public interface IPatientSearchService
    {
        List<PatientDataModel> Search(ISearchStrategy searchStrategy, PatientDataModel patient);
    }

 public class PatientSearchService :IPatientSearchService
    {
        private IPatientDataRepository _repository;

        public PatientSearchService(IPatientDataRepository repository)
        {
            _repository = repository;
        }

        public List<PatientDataModel> Search(ISearchStrategy searchStrategy, PatientDataModel patient)
        {
            var patients = _repository.GetAllPatients();
            return searchStrategy.Search(patients, patient);
        }
    }

 public interface IPatientDataRepository
    {
        List<PatientDataModel> GetAllPatients();
    }

 public class PatientDataRepository : IPatientDataRepository
    {
        public List<PatientDataModel> GetAllPatients()
        {
            List<PatientDataModel> patients = new List<PatientDataModel>();
            patients.Add(new PatientDataModel { Name = "test", EmailId = "abc@xyz.com", Mrn = "123" });
            return patients;
        }
    }

public interface ISearchStrategy
    {
        void Add(ISearchStrategy strategy);

        void Remove(ISearchStrategy strategy);

        List<PatientDataModel> Search(List<PatientDataModel> patients, PatientDataModel patient);
    }

 public class SearchStrategy : ISearchStrategy
    {
        List<ISearchStrategy> strategies;
        public void Add(ISearchStrategy strategy)
        {
            strategies.Add(strategy);
        }

        public void Remove(ISearchStrategy strategy)
        {
            strategies.Remove(strategy);
        }

        List<PatientDataModel> ISearchStrategy.Search(List<PatientDataModel> patients, PatientDataModel patient)
        {
            foreach (var strategy in strategies)
            {
                // call strategy.Search(patients, patient);
                //Return patients
            }
            return null;
        }
    }

 public class SearchByMRNStrategy : ISearchStrategy
    {
        public void Add(ISearchStrategy strategy)
        {
            throw new NotImplementedException();
        }

        public void Remove(ISearchStrategy strategy)
        {
            throw new NotImplementedException();
        }

        public List<PatientDataModel> Search(List<PatientDataModel> patients, PatientDataModel patient)
        {
            //Iplementation
            return null;
        }
    }

public class SearchByNameStrategy : ISearchStrategy
    {
        public void Add(ISearchStrategy strategy)
        {
            throw new NotImplementedException();
        }

        public void Remove(ISearchStrategy strategy)
        {
            throw new NotImplementedException();
        }

        public List<PatientDataModel> Search(List<PatientDataModel> patients, PatientDataModel patient)
        {
            //Iplementation
            return null;
        }
    }

  public static void Main(string[] args)
        {
            IPatientSearchService service = new PatientSearchService(new PatientDataRepository);
            ISearchStrategy composite = new SearchStrategy();
            composite.Add(new SearchByMRNStrategy());
            composite.Add(new SearchByNameStrategy());
            service.Search(composite, new PatientDataModel { Name = "test" });
        }
