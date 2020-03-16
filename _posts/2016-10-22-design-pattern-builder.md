---
layout: post
title:  "Design Pattern : Builder Design Pattern"
author: prashant
categories: [ coding, development, design pattern ]
---


Builder design pattern in Java is a creational pattern i.e. used to create objects, similar to factory method design pattern which is also creational design pattern.

What problem Builder pattern solves in Java
Builder pattern is a creational design pattern it means its solves problem related to object creation. Constructors in Java are used to create object and can take parameters required to create object. Problem starts when an Object can be created with lot of parameters, some of them may be mandatory and others may be optional.

Guidelines for Builder design pattern in Java

1) Make a static nested class called Builder inside the class whose object will be build by Builder. In this example its Alert Dialog.

2) Builder class will have exactly same set of fields as original class.

3) Builder class will expose method for adding properties for alert dialog e.g. title, message in this example. each method will return same Builder object. Builder will be enriched with each method call.

4) Builder.build() method will copy all builder field values into actual class and return object of Item class.

5) Item class (class for which we are creating Builder) should have private constructor to create its object from build() method and prevent outsider to access its constructor.
Example:
 I have implemented AlertDialog concept using Builder pattern.

 ```
public class AlertDialog {

   // All final Attributes
   private final String title;  // Optional
   private final String subTitle; // Optional
   private final String message; // Optional
   private final String firstButtonText; // Optional
   private final String secondButtonText; // Optional

   private AlertDialog(Builder builder) {

         this.title = builder.title;
         this.subTitle = builder.subTitle;
         this.message = builder.message;
         this.firstButtonText = builder.firstButtonText;
         this.secondButtonText = builder.secondButtonText;
   }

   public String getTitle() {
      return title;
   }
   public String getSubTitle() {
      return subTitle;
   }
   public String getMessage() {
      return message;
   }
   public String getFirstButtonText() {
      return firstButtonText;
   }
   public String getSecondButtonText() {
      return secondButtonText;
   }


   public static class Builder {

      // All final Attributes
      private String title = "";  // Optional
      private String subTitle = ""; // Optional
      private String message = ""; // Optional
      private String firstButtonText = ""; // Optional
      private String secondButtonText = ""; // Optional

      public Builder setTitle(String title) {
         this.title = title;
         return this;
      }

      public Builder setSubTitle(String subtitle) {
         this.subTitle = subtitle;
         return this;
      }

      public Builder setMessage(String message) {
         this.message = message;
         return this;
      }

      public Builder setFirstButtonText(String text) {
         this.firstButtonText = text;
         return this;
      }

      public Builder setSecondButtonText(String text) {
         this.secondButtonText = text;
         return this;
      }

      public AlertDialog Build() {
         AlertDialog dialog = new AlertDialog(this);
         validateInputs(dialog);
         return dialog;
      }

      public void validateInputs(AlertDialog dlg) {
         // Do validation heres
      }
   }

   public void show() {

      if(title != null && title != "")
         System.out.println("Title : " + title);
      if(subTitle != null && subTitle != "")
         System.out.println("Sub Title : " + subTitle);
      if(message!= null && message!= "")
         System.out.println("Message : " + message);
      if(firstButtonText!= null && firstButtonText!= "")
         System.out.println("First Button : " + firstButtonText);
      if(secondButtonText!= null && secondButtonText!= "")
         System.out.println("Second Button : " + secondButtonText);

   }
}
```

Below is the code for creating object with builder pattern.

```
public class MainClass {

    public static void main(String[] args) {

       AlertDialog dialog = new AlertDialog.Builder()
                   .setTitle("Hello")
                   .setSubTitle("Prashant Bale")
                   .setMessage("How are you?")
                   .setFirstButtonText("Fine")
                   .setSecondButtonText("Not Good")
                   .Build();

       dialog.show();

       AlertDialog dialog2 = new AlertDialog.Builder()
            .setTitle("Hello")
            .setSubTitle("Prashant Bale")
            .Build();

       dialog2.show();

    }
}
```

Builder design pattern in Java – Pros and Cons
Live everything Builder pattern also has some disadvantages, but if you look at below, advantages clearly outnumber disadvantages of Builder design pattern. Any way here are few advantages and disadvantage of Builder design pattern for creating objects in Java.



Advantages:

1) more maintainable if number of fields required to create object is more than 4 or 5.

2) less error-prone as user will know what they are passing because of explicit method call.

3) more robust as only fully constructed object will be available to client.



Disadvantages:

1) verbose and code duplication as Builder needs to copy all fields from Original or Item class.



Reference link:
http://javarevisited.blogspot.in/2012/06/builder-design-pattern-in-java-example.html
