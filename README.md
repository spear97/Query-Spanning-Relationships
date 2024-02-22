# Query Spanning Relationships

This Django project is a simple learning platform where users can enroll in courses, taught by instructors, and access lessons. It includes models for users (both instructors and learners), courses, enrollments, and lessons.

## Installation
  1. Clone the repository:
     ```bash
     git clone https://github.com/yourusername/django-learning-platform.git
     ```
  2. Create a virtual environment:
     ```bash
    cd django-learning-platform
    python3 -m venv venv
     ```
  3. Activate the virtual environment:
     - On Windows:
        ```bash
        venv\Scripts\activate
         ```
     - On macOS/Linux:
       ```bash
       source venv/bin/activate
       ```
  4. Install dependencies:
     ```bash
     pip install -r requirements.txt
     ```
     
## Database Setup
  1. Make sure you have PostgreSQL installed.
  2. Create a PostgreSQL database with the name `django_learning_platform`.
  3. Update `settings.py`:
     ```bash
           DATABASES = {
          'default': {
              'ENGINE': 'django.db.backends.postgresql',
              'NAME': 'django_learning_platform',
              'USER': 'your_username',
              'PASSWORD': 'your_password',
              'HOST': 'localhost',
              'PORT': '5432',
          }
      }
     ```
  4. Run migrations:
     ```bash
     python manage.py migrate
     ```
## Usage
  1. Create initial data:
     ```bash
     python manage.py shell < populate_data.py
     ```
  2. Start the development server:
     ```bash
     python manage.py runserver
     ```
  3. Open your browser and go to http://localhost:8000/ to access the platform.
     
## Models

### User
- Fields:
  - `first_name`: First name of the user.
  - `last_name`: Last name of the user.
  - `dob`: Date of birth of the user.
    
### Instructor (Inherits from User)
- Additional Fields:
  - `full_time`: Boolean indicating if the instructor is full-time.
  - `total_learners`: Total number of learners taught by the instructor.
    
### Learner (Inherits from User)
- Additional Fields:
  - `occupation`: Occupation of the learner.
  - `social_link`: Link to the learner's social profile.

### Course
- Fields:
  - `name`: Name of the course.
  - `description`: Description of the course.
  - `instructors`: Many-to-Many relationship with Instructor.
  - `learners`: Many-to-Many relationship with Learner through Enrollment.

### Enrollment
- Fields:
  - `learner`: Foreign key to Learner.
  - `course`: Foreign key to Course.
  - `date_enrolled`: Date when the learner enrolled.
  - `mode`: Mode of enrollment (Audit or Honor).

### Lesson
- Fields:
  - `title`: Title of the lesson.
  - `content`: Content of the lesson.
  - `course`:  Foreign key to Course.
    
## Examples
1. Get courses taught by Instructor 'Yan'
   ```python
   courses = Course.objects.filter(instructors__first_name='Yan')
   print(courses)
   ```
2. Get all learners for the course 'Introduction to Python'
   ```python
   course = Course.objects.get(name='Introduction to Python')
   learners = course.learners.all()
   print(learners)
   ```
3. Check which courses developers are enrolled in Aug 2020
   ```python
   enrollments = Enrollment.objects.filter(date_enrolled__month=8, date_enrolled__year=2020, learner__occupation='developer')
   courses_for_developers = set()
   for enrollment in enrollments:
      course = enrollment.course
      courses_for_developers.add(course.name)
   print(courses_for_developers)
   ```
