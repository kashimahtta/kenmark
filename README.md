# kenmark
project file

import MODEL from "../utils/allModels.js";

export const addStudent = async (req, res) => {
  const { schoolId, teacherId, name, email, phonenumber } = req.body;

  if (!name || !email || !phonenumber || !teacherId || !schoolId)
    return res.status(400).send("❌ data required.");
  try {
    const s = await MODEL.STUDENT.create({
      name,
      email,
      phonenumber,
      teacherId,
      schoolId,
    });

    res.status(200).json({
      message: "student added successfully",
      s,
    });
  } catch (err) {
    res.status(500).send("❌ Error adding student");
  }
};

export const getStudent = async (req, res) => {
  try {
    const student = await MODEL.STUDENT.findAll();
    res.status(200).json({
      message: "student fetch successfully",
      student,
    });
  } catch (err) {
    res.status(500).send("❌ Error fetching student");
  }
};

export const getStudentbyid = async (req, res) => {
  try {
    const student = await MODEL.STUDENT.findOne({
      where: { id: req.query.id },
      include: { model: MODEL.TEACHER, include: { model: MODEL.SCHOOL } },
    });
    res.status(200).json({
      message: "student fetch successfully",
      student,
    });
  } catch (err) {
    res.status(500).send("❌ Error fetching student");
  }
};

export const addBulkStudent = async (req, res) => {
  try {
    const data = req.body;
    const s = await MODEL.STUDENT.bulkCreate(data);

    res.status(200).json({
      message: "students added successfully",
      s,
    });
  } catch (err) {
    res.status(500).send("❌ Error adding student");
  }
};
