const { validationResult } = require("express-validator");
const HttpError = require("../models/http-error");
const Blog = require("../models/blog");

// Retrieve blog post by ID
const getBlogById = async (req, res, next) => {
  const blogId = req.params.blogId;

  let blog;
  try {
    blog = await Blog.findById(blogId);
  } catch (err) {
    return next(new HttpError("Could not find a blog for the provided id.", 500));
  }

  if (!blog) {
    return next(new HttpError("No blog found for the provided id.", 404));
  }

  res.json({ blog: blog.toObject({ getters: true }) });
};

// Create a new blog post
const createBlog = async (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return next(new HttpError("Invalid input, please check your data.", 422));
  }

  const { title, content, author } = req.body;

  const createdBlog = new Blog({
    title,
    content,
    author,
    date: new Date(),
  });

  try {
    await createdBlog.save();
  } catch (err) {
    return next(new HttpError("Creating blog failed, please try again.", 500));
  }

  res.status(201).json({ blog: createdBlog });
};

// Get all blog posts
const getAllBlogs = async (req, res, next) => {
  let blogs;
  try {
    blogs = await Blog.find();
  } catch (err) {
    return next(new HttpError("Fetching blogs failed, please try again.", 500));
  }

  res.json({ blogs: blogs.map(blog => blog.toObject({ getters: true })) });
};

// Update a blog post
const updateBlog = async (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return next(new HttpError("Invalid input, please check your data.", 422));
  }

  const { title, content } = req.body;
  const blogId = req.params.blogId;

  let blog;
  try {
    blog = await Blog.findById(blogId);
    if (!blog) {
      return next(new HttpError("Blog not found.", 404));
    }

    blog.title = title;
    blog.content = content;
    await blog.save();
  } catch (err) {
    return next(new HttpError("Updating blog failed, please try again.", 500));
  }

  res.status(200).json({ blog: blog.toObject({ getters: true }) });
};

// Delete a blog post
const deleteBlog = async (req, res, next) => {
  const blogId = req.params.blogId;

  try {
    await Blog.findByIdAndRemove(blogId);
  } catch (err) {
    return next(new HttpError("Deleting blog failed, please try again.", 500));
  }

  res.status(200).json({ message: "Blog deleted." });
};

exports.getBlogById = getBlogById;
exports.createBlog = createBlog;
exports.getAllBlogs = getAllBlogs;
exports.updateBlog = updateBlog;
exports.deleteBlog = deleteBlog;