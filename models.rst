Models Cheatsheet
-----------------

Creating abstract models 

.. code-block:: python

    import abc
    from django.db import models 
    from django.utils.translation import ugettext_lazy as _

    class AbstractTimeStampModel(models.Model):
    """TimeStampModel that holds created_date and updated_date field"""
    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    created_date = models.DateTimeField(_("Created date"), auto_now_add=True)
    updated_date = models.DateTimeField(_("Updated date"), auto_now=True)

    @abc.abstractmethod
    def __str__(self):
        """
        This method must be implemented, otherwise it will throw an error in child
        """
        pass
        
    

    class Meta:
        abstract = True
        
  
  
creating datbase models with custom manager

.. code-block:: python


    class DepartmentManager(models.Manager):
    def get_queryset(self):
        # Return only active department
        return super(FeaturedItemManager, self).get_queryset().filter(active="yes")


    class Person(AbstractTimeStampModel):
        
        first_name = models.CharField(max_length=255, verbose_name=_("Link URL"))
        last_name = models.CharField(max_length=255, verbose_name=_("Your name"))
        photo  = models.ImageField(
            upload_to="uploads/thumbnails/document/%Y/%m/%d",
            max_length=255,
            blank=True,
            help_text=_("maximum size of thumbnail should be 255px by 300px")
         )
         
        departments = DepartmentManager()
         
    
        def get_admin_url(self):
            """
            Method, return django admin url
            """ 
            return urlresolvers.reverse("admin:%s_%s_change" %(self._meta.app_label, self._meta.model_name), args=(self.pk,)
            
        def get_absolute_url(self):
            """
            Return reverse url
            """
            from django.urls import reverse
            return reverse("author:author_detail", kwargs={"author_name": slugify(self.name), "pk": self.pk})
   
        @property
        def get_name(self):
            return self.first_name + self.last_name




  
