---
layout: post
title:  "Strategy Design Pattern using Python"
date:   2016-04-12 22:48:36 -0600
categories:
---
<style>
p {
  text-align: justify
}</style>
Strategy design pattern is used, when there is a need to apply different algorithms or methods on a single set of data. The example that I would be using is a real-world example. Imagine yourself using an application in an iPhone. In this case - let it be a Expense calculation application. Suppose, there is an option to export you expenses into a JSON or XLS format. Then both these formats would depend on data from the same query set. I am providing snippets of code from one such great Application :P that I programmed using Django framework. So, let us get into the elements of the Strategy Pattern.

There are three main parts of a stratergy pattern. The first one is called the Stratergy Interface. In Java examples, we can create an interface using `public nameOfInterface interface`. However, in Python, we have to import a module called the `abc`. This stands for abstract base class (Import is done using `from abc import ABCMeta`).

Now you can go ahead by using the `__metaclass__ = ABCMeta` below the class, which you want to declare as an interface. This would make the class an interface. This class then consists of abstract methods - remember, an interface often cosists of abstract methods. In JAVA, the methods inside an interface are by default abstract. in Python, you have to force the method to behave as an abstract method. This can be done using the `@abstractmethod` code above the method, that you want to declare as an abstract class.


{% highlight ruby %}
# This is the strategy class
class ExportStrategy():
# Class declared as an interface using Python ABC module
        __metaclass__ = ABCMeta
        @abstractmethod
        def export_method(self): pass
{% endhighlight %}

Now, that we got some light on how the interface is declared along with the abstract method, we would see how this abstract method actually works. The abstract method would usually be a command method that has be later implemented in the abstract concrete classes(those classes that extend the interface). In our case, it is simpky the export method.

{% highlight ruby %}
#class for strategy 1 - Exporting in XL
class ExportXLS(ExportStrategy):
        def export_method(self,request):
                response = HttpResponse(content_type='text/xls')
                response['Content-Disposition'] = 'attachment; filename="somefilename.xls"'
                writer = csv.writer(response)
                writer.writerow([
                        smart_str(u"title"),
                        smart_str(u"category"),
                        ])
                querylist = Expenses.objects.all().order_by("-date")
                for obj in querylist:
                        writer.writerow([
                                smart_str(obj.title),
                                smart_str(obj.category),
                                ])

                        return response

{% endhighlight %}

Now, the above declared class is an abstract class. The astract class extends the interface. In python, we pass the intercae inside the class parantheses - `(ExportStrategy)`. Now, the abstract methods declared inside the interdace have to implemented into the class as well.
 So, for the first Export method in XLS, the function of exporting in XLS is served by method one.

{% highlight ruby %}
#class for strategy 2 - Exporting in CSV
class ExportCSV(ExportStrategy):
        def export_method(self,request):
                response = HttpResponse(content_type='text/csv')
                response['Content-Disposition'] = 'attachment; filename="somefilename.csv"'
                writer = csv.writer(response)
                writer.writerow([
                        smart_str(u"title"),
                        smart_str(u"category"),
                        ])
                querylist = Expenses.objects.all().order_by("-date")
                for obj in querylist:
                        writer.writerow([
                                smart_str(obj.title),
                                smart_str(obj.category),
                                ])
                                   return response

{% endhighlight %}

Similar, to that of the first strategy class, we also have the second strategy class. THis time, it is the CSV format. I hope that it is quite clear how these three parts of the pattern are working. Now, all we need is an controller. Depending on the system, there are quite some methods to implement the constoller.

I am going to leave it to you people to decide, how this is going to work. What a controller should contain is well defined - in this case, there should be an instantiation of our 2 Concrete classes and the metjod call for the corresponding methods.

That is the Strategy pattern. It is quite straight forward and one of the easiest patterns out there. Hoping to keep you posted with another pattern soon!

<br>
<b>Do you find any typos or disagree with something on this page? Let me know!</b>
