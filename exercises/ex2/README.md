# Exercise 2 - Provision Your Multi-tenant Solution to Your Customer

In this exercise, you will create...

## Exercise 2.1 Sub Exercise 1 Description

After completing these steps you will have created...

1. Click here.
<br>![](/exercises/ex2/images/02_01_0010.png)

2.	Insert this line of code.
```abap
response->set_text( |Hello ABAP World! | ). 
```



## Exercise 2.2 Sub Exercise 2 Description

After completing these steps you will have...

1.	Enter this code.
```abap
DATA(lt_params) = request->get_form_fields(  ).
READ TABLE lt_params REFERENCE INTO DATA(lr_params) WITH KEY name = 'cmd'.
  IF sy-subrc = 0.
    response->set_status( i_code = 200
                     i_reason = 'Everything is fine').
    RETURN.
  ENDIF.

```

2.	Click here.
<br>![](/exercises/ex2/images/02_02_0010.png)

## Summary

You've now ...

Continue to - [Exercise 3 - Excercise 3 ](../ex3/README.md)


[Exercise 1 - Provision your multi-tenant solution to your customer](exercises/ex1/README.md)
    - [Exercise 1.1 - Subscribe the solution in the application subscription of your customer](exercises/ex1#exercise-11-sub-exercise-1-description)
    - [Exercise 1.2 - Configure the connection to the Cloud ERP](exercises/ex1#exercise-12-sub-exercise-2-description)
    - [Exercise 1.3 - Configure SAP Build Work Zone](exercises/ex1#exercise-12-sub-exercise-2-description)