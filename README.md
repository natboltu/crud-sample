
Given the two collections `users` and `courses`. And another sub-schemas of `lectureSchema`, `fileSchema`, and `questionSchema`.

```typescript
import { Schema, model, InferSchemaType } from "mongoose";

const userSchema = new Schema(
  {
    fullname: { type: String, required: true },
    email: { type: String, unique: true, required: true },
    password: { type: String, required: true },
    location: { type: [Number], required: false },
    hobbies: { type: [String], required: false },
  },
  { versionKey: false }
);

const courseSchema = new Schema(
  {
    code: { type: String, required: true },
    title: { type: String, required: true },
    created_by: {
      user_id: Schema.Types.ObjectId,
      fullname: String,
      email: String,
    },
    lectures: [lectureSchema],
  },
  { versionKey: false }
);

const lectureSchema = new Schema({
  title: { type: String, required: true },
  description: { type: String, required: true },
  files_url: [fileSchema],
  questions: [questionSchema],
});

const fileSchema = new Schema({
  originalname: { type: String, required: true },
  mimetype: { type: String, required: true },
  path: { type: String, required: true },
  size: { type: Number, required: true },
});

const questionSchema = new Schema({
  question: { type: String, required: true },
  due_date: { type: Number, default: () => Date.now() + 86400000 },
});

export type User = InferSchemaType<typeof userSchema>;
export type Course = InferSchemaType<typeof courseSchema>;
export type Lecture = InferSchemaType<typeof lectureSchema>;
export type File = InferSchemaType<typeof fileSchema>;
export type Question = InferSchemaType<typeof questionSchema>;

export default model<User>("user", userSchema);
export default model<Course>("course", courseSchema);
```

<ins>Implement the following methods:</ins>

- add a new user with (`fullname`, `email`, `password`, and `location` with the following value `[-91.96731488465576, 41.018654231616374]`)
- add a new course with (`code`, `title`). Fill out the `created_by` property from the previous method (prefixed hard-coded values).
- add a new lecture with (`title`, `description`).
- add a new question with (`question`).
- use the aggregation pipeline to get all questions for a specific course and lecture. (with pagination).
- use the aggregation pipeline to get one question by ID, for a given course ID and lecture ID.
- update a question by ID, for a given course ID and lecture ID, and extend the due date for another day.
- update all questions questions, for a given course ID and lecture ID, and extend the due date for another day.
- delete one question by ID, for a given course ID and lecture ID.
- find the nearest 10 users that match a certain set of hobbies, using `2d` index.
