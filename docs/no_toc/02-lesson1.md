# Intro to Computing

## Goals of the course

-   Fundamental concepts in high-level programming languages (R, Python, Julia, WDL, etc.) that is transferable: *How do programs run, and how do we solve problems using functions and data structures?*

-   Beginning of data science fundamentals: *How do you translate your scientific question to a data wrangling problem and answer it?*

    ![Data science workflow](https://d33wubrfki0l68.cloudfront.net/571b056757d68e6df81a3e3853f54d3c76ad6efc/32d37/diagrams/data-science.png){width="450"}

-   Find a nice balance between the two throughout the course: we will try to reproduce a figure from a scientific publication using new data.

## What is a computer program?

-   A sequence of instructions to manipulate data for the computer to execute.

-   A series of translations: English \<-\> Programming Code for Interpreter \<-\> Machine Code for Central Processing Unit (CPU)

We will focus on English \<-\> Programming Code for R Interpreter in this class.

More importantly: **How we organize ideas \<-\> Instructing a computer to do something**.

## A programming language has following elements: {#a-programming-language-has-following-elements}

-   Grammar structure (simple building blocks)

-   Means of combination to analyze and create content (examples around genomics provided, and your scientific creativity is strongly encouraged!)

-   Means of abstraction for modular and reusable content (data structures, functions)

-   Culture (emphasis on open-source, collaborative, reproducible code)

Requires a lot of practice to be fluent!

## What is R and why should I use it?

It is a:

-   Dynamic programming interpreter

-   Highly used for data science, visualization, statistics, bioinformatics

-   Open-source and free; easy to create and distribute your content; quirky culture

## R vs. Python as a first language

In terms of our goals, recall:

-   Fundamental concepts in high-level programming languages

-   Beginning of data science fundamentals

There are a lot of nuances and debates, but I argue that Python is a better learning environment for the former and R is better for the latter.

Ultimately, either should be okay! Perhaps more importantly, *consider what your research group and collaborator are more comfortable with*.

## Posit Cloud Setup

Posit Cloud/RStudio is an Integrated Development Environment (IDE). Think about it as Microsoft Word to a plain text editor. It provides extra bells and whistles to using R that is easier for the user.

Today, we will pay close attention to:

-   Script editor: where sequence of instructions are typed and saved as a text document as a R program. To run the program, the console will execute every single line of code in the document.

-   Console (interpreter): Instead of giving a entire program in a text file, you could interact with the R Console line by line. You give it one line of instruction, and the console executes that single line. It is what R looks like without RStudio.

-   Environment: Often, code will store information *in memory*, and it is shown in the environment. More on this later.

## Using Quarto for your work

Why should we use Quarto for data science work?

-   Encourages reproducible workflows

-   Code, output from code, and prose combined together

-   Extendability to Python, Julia, and more.

More options and guides can be found in [Introduction to Quarto](https://quarto.org/docs/get-started/hello/rstudio.html) .

## Grammar Structure 1: Evaluation of Expressions

-   **Expressions** are be built out of **operations** or **functions**.

-   Operations and functions combine **data types** to return another data type.

-   We can combine multiple expressions together to form more complex expressions: an expression can have other expressions nested inside it.

For instance, consider the following expressions entered to the R Console:


```r
18 + 21
```

```
## [1] 39
```

```r
max(18, 21)
```

```
## [1] 21
```

```r
max(18 + 21, 65)
```

```
## [1] 65
```

```r
18 + (21 + 65)
```

```
## [1] 104
```

```r
length("ATCG")
```

```
## [1] 1
```

Here, our input **data types** to the operation are **numeric** in lines 1-4 and our input data type to the function is **character** in line 5.

Operations are just functions in hiding. We could have written:


```r
sum(18, 21)
```

```
## [1] 39
```

```r
sum(18, sum(21, 65))
```

```
## [1] 104
```

Remember the function machine from algebra class? We will use this schema to think about expressions.

![Function machine from algebra class.](https://cs.wellesley.edu/~cs110/lectures/L16/images/function.png)

If an expression is made out of multiple, nested operations, what is the proper way of the R Console interpreting it? Being able to read nested operations and nested functions as a programmer is very important.


```r
3 * 4 + 2
```

```
## [1] 14
```

```r
3 * (4 + 2)
```

```
## [1] 18
```

Lastly, a note on the use of functions: a programmer should not need to know how the function is implemented in order to use it - this emphasizes [abstraction and modular thinking](#a-programming-language-has-following-elements), a foundation in any programming language.

## Grammar Structure 2: Storing data types in the global environment

To build up a computer program, we need to store our returned data type from our expression somewhere for downstream use. We can assign a variable to it as follows:


```r
x = 18 + 21
```

If you enter this in the Console, you will see that in the Environment, the variable `x` has a value of `39`.

::: callout-tip
## Execution rule for variable assignment

Evaluate the expression to the right of `=`.

Bind variable to the left of `=` to the resulting value.

The variable is stored in the environment.

`<-` is okay too!
:::

The environment is where all the variables are stored, and can be used for an expression anytime once it is defined. Only one unique variable name can be defined.

The variable is stored in the working memory of your computer, Random Access Memory (RAM). This is temporary memory storage on the computer that can be accessed quickly. Typically a personal computer has 8, 16, 32 Gigabytes of RAM. When we work with large datasets, if you assign a variable to a data type larger than the available RAM, it will not work. More on this later.

Look, now `x` can be reused downstream:


```r
x - 2
```

```
## [1] 37
```

```r
y = x * 2
```

## Grammar Structure 3: Evaluation of Functions

A function has a **function name**, **arguments**, and **returns** a data type.

::: callout-tip
## Execution rule for functions:

Evaluate the function by its arguments, and if the arguments are functions or contains operations, evaluate those functions or operations first.

The output of functions is called the **returned value**.
:::


```r
sqrt(nchar("hello"))
```

```
## [1] 2.236068
```

```r
(nchar("hello") + 4) * 2
```

```
## [1] 18
```

## Functions to read in data

We are going to read in a Comma Separated Value (CSV) spreadsheet, that contains information about cancer cell lines.

The first line calls the function `read.csv()` with a string argument representing the file path to the CSV file (we are using an URL online, but this is typically done locally), and the returned data type is stored in `metadata` variable. The resulting `metadata` variable is a new data type you have never seen before. It is a **data structure** called a **data frame** that we will be exploring next week. It holds a table of several data types that we can explore.

We run a few functions on `metadata`.


```r
metadata = read.csv("https://github.com/caalo/Intro_to_R/raw/main/classroom_data/CCLE_metadata.csv")
head(metadata)
```

```
##      ModelID PatientID CellLineName StrippedCellLineName Age SourceType
## 1 ACH-000001 PT-gj46wT  NIH:OVCAR-3            NIHOVCAR3  60 Commercial
## 2 ACH-000002 PT-5qa3uk        HL-60                 HL60  36 Commercial
## 3 ACH-000003 PT-puKIyc        CACO2                CACO2  72 Commercial
## 4 ACH-000004 PT-q4K2cp          HEL                  HEL  30 Commercial
## 5 ACH-000005 PT-q4K2cp   HEL 92.1.7              HEL9217  30 Commercial
## 6 ACH-000006 PT-ej13Dz   MONO-MAC-6             MONOMAC6  64 Commercial
##   SangerModelID      RRID DepmapModelType AgeCategory GrowthPattern
## 1     SIDM00105 CVCL_0465           HGSOC       Adult      Adherent
## 2     SIDM00829 CVCL_0002             AML       Adult    Suspension
## 3     SIDM00891 CVCL_0025            COAD       Adult      Adherent
## 4     SIDM00594 CVCL_0001             AML       Adult    Suspension
## 5     SIDM00593 CVCL_2481             AML       Adult         Mixed
## 6     SIDM01023 CVCL_1426            AMOL       Adult    Suspension
##   LegacyMolecularSubtype PrimaryOrMetastasis               SampleCollectionSite
## 1                                 Metastatic                            ascites
## 2                                    Primary haematopoietic_and_lymphoid_tissue
## 3                                    Primary                              Colon
## 4                                    Primary haematopoietic_and_lymphoid_tissue
## 5                                                                   bone_marrow
## 6                                    Primary haematopoietic_and_lymphoid_tissue
##      Sex SourceDetail  LegacySubSubtype CatalogNumber
## 1 Female         ATCC high_grade_serous        HTB-71
## 2 Female         ATCC                M3       CCL-240
## 3   Male         ATCC                          HTB-37
## 4   Male         DSMZ                M6        ACC 11
## 5   Male         ATCC                M6       HEL9217
## 6   Male         DSMZ                M5       ACC 124
##                                      CCLEName COSMICID PublicComments
## 1                             NIHOVCAR3_OVARY   905933               
## 2     HL60_HAEMATOPOIETIC_AND_LYMPHOID_TISSUE   905938               
## 3                       CACO2_LARGE_INTESTINE       NA               
## 4      HEL_HAEMATOPOIETIC_AND_LYMPHOID_TISSUE   907053               
## 5  HEL9217_HAEMATOPOIETIC_AND_LYMPHOID_TISSUE       NA               
## 6 MONOMAC6_HAEMATOPOIETIC_AND_LYMPHOID_TISSUE   908148               
##   WTSIMasterCellID EngineeredModel TreatmentStatus OnboardedMedia PlateCoating
## 1             2201                                     MF-001-041         None
## 2               55                                     MF-005-001         None
## 3               NA                         Unknown     MF-015-009         None
## 4              783                  Post-treatment     MF-001-001         None
## 5               NA                                     MF-001-001         None
## 6             2167                                     MF-001-001         None
##   OncotreeCode                      OncotreeSubtype    OncotreePrimaryDisease
## 1        HGSOC     High-Grade Serous Ovarian Cancer  Ovarian Epithelial Tumor
## 2          AML               Acute Myeloid Leukemia    Acute Myeloid Leukemia
## 3         COAD                 Colon Adenocarcinoma Colorectal Adenocarcinoma
## 4          AML               Acute Myeloid Leukemia    Acute Myeloid Leukemia
## 5          AML               Acute Myeloid Leukemia    Acute Myeloid Leukemia
## 6         AMOL Acute Monoblastic/Monocytic Leukemia    Acute Myeloid Leukemia
##        OncotreeLineage
## 1 Ovary/Fallopian Tube
## 2              Myeloid
## 3                Bowel
## 4              Myeloid
## 5              Myeloid
## 6              Myeloid
```

```r
nrow(metadata)
```

```
## [1] 1864
```

```r
ncol(metadata)
```

```
## [1] 30
```

If you don't know what a function does, ask for help:


```r
?nrow
```

## Tips on Exercises / Debugging

Common errors:

-   Syntax error.

-   Changing a variable without realizing you did so.

-   The function or operation does not accept the input data type.

-   It did something else than I expected!

Solutions:

-   Where is the problem?

-   What kind of problem is it?

-   Explain your problem to someone!
