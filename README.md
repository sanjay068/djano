Are Django signals executed synchronously or asynchronously by default?
Django signals are executed synchronously
from django.db import models
from django.db.models.signals import post_save
from django.dispatch import receiver
import time

class Book(models.Model):
    title = models.CharField(max_length=100)

@receiver(post_save, sender=Book)
def book_saved(sender, instance, **kwargs):
    print("Signal received, starting long task...")
    time.sleep(5)
    print("Signal completed")

Do Django signals run in the same thread as the caller?
Yes, signals run in the same thread as the caller by default.
import threading
from django.db import models
from django.db.models.signals import post_save
from django.dispatch import receiver

class Author(models.Model):
    name = models.CharField(max_length=100)

@receiver(post_save, sender=Author)
def check_thread(sender, instance, **kwargs):
    print("Signal thread ID:", threading.get_ident())

Do Django signals run in the same DB transaction as the caller?
Yes, by default, signals run in the same database transaction. If the transaction is rolled back, the signal effect is also discarded.

from django.db import models, transaction
from django.db.models.signals import post_save
from django.dispatch import receiver

class Log(models.Model):
    message = models.CharField(max_length=100)

class Task(models.Model):
    name = models.CharField(max_length=100)

@receiver(post_save, sender=Task)
def log_task(sender, instance, **kwargs):
    Log.objects.create(message=f"Task created: {instance.name}")

class Rectangle:
    def __init__(self, length: int, width: int):
        self.length = length
        self.width = width
        self._index = 0  # for iteration

    def __iter__(self):
        self._index = 0  # Reset each time iteration starts
        return self

    def __next__(self):
        if self._index == 0:
            self._index += 1
            return {'length': self.length}
        elif self._index == 1:
            self._index += 1
            return {'width': self.width}
        else:
            raise StopIteration()

