# Dependency Injection and Model

## Set up Student Model

1. Create a folder "/Models". Then create a class **Student** inside.

    It is not necessary to put models inside this folder, but it is a good practice.

    ```c#
    public class Student
    {
        public int Id { get; set; }
        public string Name { get; set; }
        /// <summary>
        /// Major classes
        /// </summary>
        public string Major { get; set; }

        public string Email { get; set; }
    }
    ```

2. Create a folder "/DataRepositories".

    Create an _Interface_ **IStudentRepository**.
    ```c#
    public interface IStudentRepository
    {
        Student GetStudent(int id);
    }
    ```
    Create an _service_ (class) **MockStudentRepository**, which is an implementation of interface **IStudentRepository**.
    ```c#
    public class MockStudentRepository : IStudentRepository
    {
        private List<Student> _studentList;

        public MockStudentRepository()
        {
            _studentList =  new List<Student>()
            {
                new Student() 
                { 
                    Id = 1, 
                    Name = "Bob", 
                    Major = "CS", 
                    Email = "bob@gmail.com"
                },
                new Student(),
                { 
                    Id = 2, 
                    Name = "Alice", 
                    Major = "Delivery", 
                    Email = "alice@gmail.com"
                },
                new Student(),
                { 
                    Id = 3, 
                    Name = "John", 
                    Major = "Electronic Business", 
                    Email = "john@gmail.com"
                }
            };
        }

        public Student GetStudent(int id)
        {
            return _studentList.FirstOrDefault( a => a.Id == id );
        }
    }
    ```
    In the future we just write code aiming the interface **IStudentRepository**, instead of the class MockStudentRepository. It's more flexible and easy to do unit test. This kind of abstract allows us to use **Dependency Injection**.
