---
layout: post
title:      "Javascript Project : Gradebook "
date:       2020-12-16 01:31:54 -0500
permalink:  javascript_project_gradebook
---


My JS + Rails API project is a gradebook  that keeps a record of the students grades. 
 use Javascript frontend with a Rails API backend. Client/server interaction must be handled asynchronously in JSON format.
 
 
```

  def index
    grades = Grade.all
    options = {
      # include associated category
      include: [:student]
    }
    # pass options object to serializer
    render json: GradeSerializer.new(grades, options)
  end


  
  def create
    grade = Grade.new(grade_params)
    grade.save
    render json: GradeSerializer.new(grade), status: :accepted
  end

  def destroy 
    grade = Grade.find_by(id:params[:id])
    grade.destroy
    render json: grade
  end 


```
