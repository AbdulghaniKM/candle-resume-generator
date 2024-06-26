<template>
  <main class="min-h-screen bg-white pt-2 px-4" v-if="Form">
    <div class="flex flex-col justify-center items-center mb-8 sm:text-center">
      <h1 class="font-bold text-emerald-800 text-4xl">
        Candle Resume Generator
      </h1>
      <h3 class="text-2xl font-semibold text-emerald-700">
        Welcome To Candle, Your Next Dream Job Is Waiting!
      </h3>
      <p class="text-emerald-600">
        Please fill out this form:
      </p>
    </div>

    <div class="flex flex-wrap gap-8">
      <div class="flex-1 min-w-[300px]">
        <div class="flex flex-col gap-2 text-emerald-800">
          <label>Full Name:</label>
          <input type="text" class="input h-10 bg-emerald-400" v-model="nameOfUser">
          <span v-if="errors.nameOfUser" class="text-red-500">{{ errors.nameOfUser }}</span>

          <label>Job Title:</label>
          <input type="text" class="input bg-emerald-400" v-model="jobTitle">
          <span v-if="errors.jobTitle" class="text-red-500">{{ errors.jobTitle }}</span>

          <label>About You:</label>
          <textarea v-model="aboutText" placeholder="tell us about yourself..."
            class="bg-emerald-400 placeholder:text-emerald-800 rounded-lg placeholder:pl-1 placeholder:pt-1 pl-2 pt-1"
            rows="4"></textarea>
          <span v-if="errors.aboutText" class="text-red-500">{{ errors.aboutText }}</span>

          <label>Contact</label>
          <ul>
            <li class="mb-1">
              <label>Phone:</label>
              <input type="tel" class="input h-8 bg-emerald-400 ml-5" v-model="PhoneNumber">
              <p v-if="errors.PhoneNumber" class="text-red-500">{{ errors.PhoneNumber }}</p>
            </li>
            <li class="mb-1">
              <label>Email:</label>
              <input type="email" class="input h-8 bg-emerald-400 ml-6" v-model="EmailAddress">
              <p v-if="errors.EmailAddress" class="text-red-500">{{ errors.EmailAddress }}</p>
            </li>
            <li>
              <label>Address:</label>
              <input type="text" class="input h-8 bg-emerald-400 ml-1" v-model="PhysicalAddress">
              <p v-if="errors.PhysicalAddress" class="text-red-500">{{ errors.PhysicalAddress }}</p>
            </li>
          </ul>

          <label>Skills (separated by commas):</label>
          <input type="text" class="input h-8 bg-emerald-400" v-model="Newskill">
          <button @click="addSkills" class="bg-emerald-500 text-white py-2 px-4 rounded">Add Skills</button>
          <p v-if="errors.Newskill" class="text-red-500">{{ errors.Newskill }}</p>
        </div>
      </div>

      <div class="flex-1 min-w-[300px]">
        <div class="flex flex-col gap-2 text-emerald-800">
          <label>Experience:</label>
          <div v-for="(exp, index) in experience" :key="index" class="mb-4">
            <input type="text" class="input h-8 placeholder:text-emerald-800 bg-emerald-400 mb-2 mr-3"
              v-model="exp.title" placeholder="Job Title">
            <p v-if="errors['experience_' + index + '_title']" class="text-red-500">{{ errors['experience_' + index +
              '_title'] }}</p>
            <input type="text" class="input h-8 placeholder:text-emerald-800 bg-emerald-400 mb-2" v-model="exp.company"
              placeholder="Company">
            <p v-if="errors['experience_' + index + '_company']" class="text-red-500">{{ errors['experience_' + index +
              '_company'] }}</p>
            <input type="text" class="input h-8 placeholder:text-emerald-800 bg-emerald-400 mb-2 mr-3"
              v-model="exp.location" placeholder="Location">
            <input type="date" class="input h-8 placeholder:text-emerald-800 bg-emerald-400 mb-2" v-model="exp.date"
              placeholder="Date">
            <p v-if="errors['experience_' + index + '_date']" class="text-red-500">{{ errors['experience_' + index +
              '_date'] }}</p>
            <textarea v-model="exp.description" placeholder="Job description..."
              class="bg-emerald-400 placeholder:text-emerald-800 rounded-lg placeholder:pl-1 placeholder:pt-1 pl-2 pt-1 w-full"
              rows="4"></textarea>
            <p v-if="errors['experience_' + index + '_description']" class="text-red-500">{{ errors['experience_' +
              index +
              '_description'] }}</p>
            <button @click="deleteExperience(index)" class="bg-red-500 text-white py-1 px-2 rounded">Delete</button>
          </div>
          <button @click="addExperience" class="bg-emerald-500 text-white py-2 px-4 rounded">Add Experience</button>

          <label>Education:</label>
          <div v-for="(edu, index) in education" :key="index" class="mb-4">
            <input type="text" class="input h-8 placeholder:text-emerald-800 bg-emerald-400 mb-2 mr-3"
              v-model="edu.degree" placeholder="Degree">
            <p v-if="errors['education_' + index + '_degree']" class="text-red-500">{{ errors['education_' + index +
              '_degree'] }}</p>
            <input type="text" class="input h-8 placeholder:text-emerald-800 bg-emerald-400 mb-2 "
              v-model="edu.university" placeholder="University">
            <input type="date" class="input h-8 placeholder:text-emerald-800 bg-emerald-400 mb-2 ml-32 sm:ml-0"
              v-model="edu.date" placeholder="Date"><br>
            <button @click="deleteEducation(index)" class="bg-red-500 text-white py-1 px-2 rounded">Delete</button>
          </div>
          <button @click="addEducation" class="bg-emerald-500 text-white py-2 px-4 rounded">Add Education</button>
        </div>
      </div>
    </div>

    <div class="mt-8 text-center">
      <button @click="validateForm" class="bg-emerald-500 text-white py-2 px-4 rounded">
        Show Preview
      </button>
    </div>
  </main>
  <div v-if="ShowPrev" class="min-h-screen bg-white pt-2 px-4">
    <header class="flex items-center justify-between p-4 bg-gray-100">
      <div>
        <h1 class="text-3xl font-bold">{{ name }}</h1>
        <h2 class="text-xl font-semibold">{{ title }}</h2>
      </div>
    </header>
    <div class="flex p-4">
      <aside class="w-1/3 pr-4">
        <section class="mb-4">
          <h4 class="text-2xl font-semibold">{{ headlines[0] }}</h4>
          <p class="text-gray-700">
            {{ IntroText }}
          </p>
        </section>
        <section class="mb-4">
          <h4 class="text-2xl font-semibold">{{ headlines[1] }}</h4>
          <ul class="text-gray-700">
            <li>{{ PhoneNumber }}</li>
            <li>{{ EmailAddress }}</li>
            <li>{{ PhysicalAddress }}</li>
          </ul>
        </section>
        <section class="mb-4">
          <h4 class="text-2xl font-semibold">{{ headlines[2] }}</h4>
          <ul class="text-gray-700">
            <li v-for="(skill, index) in skills" :key="index">
              {{ skill }}
            </li>
          </ul>
        </section>

      </aside>
      <section class="w-2/3">
        <div v-if="experiance.length > 0">
          <h4 class="text-2xl font-semibold">{{ headlines[3] }}</h4>
          <div class="mt-4" v-for="(item, index) in experiance" :key="index">
            <h5 class="font-semibold">{{ item.title }}</h5>
            <h6 class="italic">{{ item.company }} | {{ item.date }}</h6>
            <ul class="list-inside text-gray-700">
              <li>
                {{ item.description }}
              </li>
            </ul>
          </div>
        </div>
        <div v-if="education.length > 0">
          <h4 class="text-2xl font-semibold mt-4">{{ headlines[4] }}</h4>
          <div v-for="(item, index) in education" :key="index">
            <h5 class="font-semibold text-gray-700">{{ item.degree }}</h5>
            <h6 class="italic">{{ item.university }}</h6>
            <h6 class="italic">Graduation Date: {{ item.date }}</h6>
          </div>
        </div>
      </section>

    </div>
    <div class="flex justify-center items-center">
      <button @click="ShowForm" class="bg-gray-800 text-white py-2 px-4 rounded">
        Back
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      Form: true,
      ShowPrev: false,
      nameOfUser: '',
      jobTitle: '',
      aboutText: '',
      PhoneNumber: '',
      EmailAddress: '',
      PhysicalAddress: '',
      Newskill: '',
      name: '',
      title: '',
      IntroText: '',
      errors: {},
      contact: {
        phone: '',
        email: '',
        address: '',
      },
      headlines: ["About Me", "Contact", "Skills", "Experience", "Education"],
      skills: [],
      experience: [
        {
          title: "",
          company: "",
          location: "",
          date: "",
          description: ""
        }
      ],
      education: [
        {
          degree: "",
          university: "",
          date: ""
        },
      ]
    }
  },
  methods: {
    ShowForm() {
      this.Form = true;
      this.ShowPrev = false;
    },
    addEducation() {
      this.education.push({
        degree: "",
        university: "",
        date: ""
      });
    },
    deleteEducation(index) {
      this.education.splice(index, 1);
    },
    addExperience() {
      this.experience.push({
        title: "",
        company: "",
        location: "",
        date: "",
        description: ""
      });
    },
    deleteExperience(index) {
      this.experience.splice(index, 1);
    },
    validateForm() {
      this.errors = {};

      if (!this.nameOfUser) {
        this.errors.nameOfUser = 'Full Name is required.';
      }

      if (!this.jobTitle) {
        this.errors.jobTitle = 'Job Title is required.';
      }

      if (!this.aboutText) {
        this.errors.aboutText = 'About You is required.';
      }

      if (!this.PhoneNumber) {
        this.errors.PhoneNumber = 'Phone Number is required.';
      } else if (!/^((07[7-8]\d{8})|(\+964\d{10}))$/.test(this.PhoneNumber)) {
        this.errors.PhoneNumber = 'Invalid Phone Number format. It should be 077/078 followed by 8 digits, or +964 followed by 10 digits.';
      }

      if (!this.EmailAddress) {
        this.errors.EmailAddress = 'Email Address is required.';
      } else if (!/\S+@\S+\.\S+/.test(this.EmailAddress)) {
        this.errors.EmailAddress = 'Invalid Email format.';
      }

      if (!this.PhysicalAddress) {
        this.errors.PhysicalAddress = 'Address is required.';
      }

      this.experience.forEach((exp, index) => {
        if (!exp.title) this.errors[`experience_${index}_title`] = 'Job Title is required.';
        if (!exp.company) this.errors[`experience_${index}_company`] = 'Company is required.';
        if (!exp.date) this.errors[`experience_${index}_date`] = 'Date is required.';
        if (!exp.description) this.errors[`experience_${index}_description`] = 'Job description is required.';
      });
      this.education.forEach((edu, index) => {
        if (!edu.degree) this.errors[`education_${index}_degree`] = 'Degree is required.';
        if (!edu.university) this.errors[`education_${index}_university`] = 'University is required.';
        if (!edu.date) this.errors[`education_${index}_date`] = 'Date is required.';
      });


      if (Object.keys(this.errors).length === 0) {
        this.Form = false
        this.PreviewShown();
      }
    },
    addSkills() {
      if (!this.Newskill) {
        this.errors.Newskill = 'Skills are required.';
        return;
      }
      this.skills = this.skills.concat(this.Newskill.split(',').map(skill => skill.trim()));
      this.Newskill = '';
      this.errors.Newskill = '';
    },
    PreviewShown() {
      this.name = this.nameOfUser;
      this.title = this.jobTitle;
      this.IntroText = this.aboutText;
      this.contact.phone = this.PhoneNumber;
      this.contact.email = this.EmailAddress;
      this.contact.address = this.PhysicalAddress;
      this.experiance = this.experience;
      this.ShowPrev = true;
    }
  }
};
</script>

<style scoped>
h4 {
  margin-bottom: 1rem;
  margin-top: 1rem;
}
</style>
