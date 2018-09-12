Models Cheatsheet
-----------------

Creating abstract models 

.. code-block:: python
   :emphasize-lines: 3,5
   
    from django.db import models
   
    class TimeStampedModel(models.Model):
        created_date = models.DateTimeField(auto_now_add=True)
        updated_date = model.DateTimeField(auto_now=True)

        class Meta:
            abstract = True  


  
