# Algebra



## Key mathematical ideas

In this chapter, we study four connected algebraic ideas:

- the modulus function
- solving modulus equations and inequalities
- polynomial division
- the factor theorem and the remainder theorem

The aim is to connect exact algebraic reasoning with graphical interpretation.

Algebra is not only a collection of techniques. It is a language for describing structure. In this chapter, we use graphs to support algebraic thinking, especially when deciding how many solutions an equation has and where those solutions are located.

## The modulus function

The modulus of a real number is its distance from zero on the number line.

$$
|x| =
\begin{cases}
x, & x \geq 0,\\
-x, & x < 0.
\end{cases}
$$

Therefore, $|x|$ is always non-negative.

For example,

$$
|3|=3,\qquad |-3|=3.
$$

This means that $|x|$ does not record direction. It records distance.

## Visualisation using R: the graph of $y=|x|$


::: {.cell}

```{.r .cell-code}
df <- data.frame(
  x = seq(-5, 5, length.out = 500)
)

df$y <- abs(df$x)

ggplot(df, aes(x = x, y = y)) +
  geom_line(linewidth = 1.1) +
  geom_hline(yintercept = 0) +
  geom_vline(xintercept = 0) +
  coord_equal() +
  labs(
    title = "Graph of y = |x|",
    subtitle = "The graph changes rule at x = 0.",
    x = "x",
    y = "y"
  ) +
  book_theme()
```

::: {.cell-output-display}
![The graph of y = |x|.](index_files/figure-html/fig-modulus-basic-1.png){#fig-modulus-basic width=672}
:::
:::


The graph has a sharp corner at the origin because the algebraic rule changes at $x=0$.

For $x \geq 0$,

$$
|x|=x.
$$

For $x<0$,

$$
|x|=-x.
$$

## Transforming modulus graphs

Consider the function

$$
y=|2x-3|.
$$

The expression inside the modulus changes sign when

$$
2x-3=0.
$$

Solving gives

$$
x=\frac{3}{2}.
$$

So the turning point of the graph occurs at

$$
\left(\frac{3}{2},0\right).
$$


::: {.cell}

```{.r .cell-code}
df <- data.frame(
  x = seq(-2, 5, length.out = 500)
)

df$y <- abs(2 * df$x - 3)

ggplot(df, aes(x = x, y = y)) +
  geom_line(linewidth = 1.1) +
  geom_hline(yintercept = 0) +
  geom_vline(xintercept = 0) +
  geom_vline(xintercept = 3/2, linetype = "dashed") +
  annotate("point", x = 3/2, y = 0, size = 3) +
  annotate("text", x = 3/2, y = 0.7, label = "turning point") +
  labs(
    title = "Graph of y = |2x - 3|",
    subtitle = "The turning point occurs where 2x - 3 = 0.",
    x = "x",
    y = "y"
  ) +
  book_theme()
```

::: {.cell-output-display}
![The graph of y = |2x - 3|.](index_files/figure-html/fig-modulus-transformed-1.png){#fig-modulus-transformed width=672}
:::
:::


## Worked example 1: solving a modulus equation

Solve

$$
|2x-3|=5.
$$

Since $|A|=5$ means $A=5$ or $A=-5$, we split the equation into two cases.

First,

$$
2x-3=5.
$$

Therefore,

$$
2x=8,
$$

so

$$
x=4.
$$

Second,

$$
2x-3=-5.
$$

Therefore,

$$
2x=-2,
$$

so

$$
x=-1.
$$

Hence the solutions are

$$
x=4\quad \text{or}\quad x=-1.
$$

We can check both values using R.


::: {.cell}

```{.r .cell-code}
solutions <- c(4, -1)
abs(2 * solutions - 3)
```

::: {.cell-output .cell-output-stdout}

```
[1] 5 5
```


:::
:::


Both values give $5$, so both solutions are valid.

## Worked example 2: solving a modulus equation with checking

Solve

$$
|x-4|=2x+1.
$$

We split into two possible cases.

Case 1:

$$
x-4=2x+1.
$$

Then

$$
x=-5.
$$

Case 2:

$$
x-4=-(2x+1).
$$

Then

$$
x-4=-2x-1,
$$

so

$$
3x=3,
$$

and therefore

$$
x=1.
$$

However, modulus equations of this type require checking.

For $x=-5$,

$$
|x-4|=|-9|=9,
$$

but

$$
2x+1=2(-5)+1=-9.
$$

So $x=-5$ is not valid.

For $x=1$,

$$
|x-4|=|-3|=3,
$$

and

$$
2x+1=3.
$$

So $x=1$ is valid.

Therefore, the solution is

$$
x=1.
$$


::: {.cell}

```{.r .cell-code}
candidate_solutions <- c(-5, 1)

data.frame(
  x = candidate_solutions,
  left_side = abs(candidate_solutions - 4),
  right_side = 2 * candidate_solutions + 1,
  valid = abs(candidate_solutions - 4) == 2 * candidate_solutions + 1
)
```

::: {.cell-output .cell-output-stdout}

```
   x left_side right_side valid
1 -5         9         -9 FALSE
2  1         3          3  TRUE
```


:::
:::


## Visualisation using R: checking intersections

The equation

$$
|x-4|=2x+1
$$

can be interpreted as the intersection of two graphs:

$$
y=|x-4|
$$

and

$$
y=2x+1.
$$


::: {.cell}

```{.r .cell-code}
df <- data.frame(
  x = seq(-6, 6, length.out = 600)
)

df$modulus <- abs(df$x - 4)
df$linear <- 2 * df$x + 1

ggplot(df, aes(x = x)) +
  geom_line(aes(y = modulus, linetype = "y = |x - 4|"), linewidth = 1.1) +
  geom_line(aes(y = linear, linetype = "y = 2x + 1"), linewidth = 1.1) +
  geom_hline(yintercept = 0) +
  geom_vline(xintercept = 0) +
  annotate("point", x = 1, y = 3, size = 3) +
  annotate("text", x = 1.5, y = 3.5, label = "valid intersection") +
  labs(
    title = "Graphical solution of |x - 4| = 2x + 1",
    subtitle = "The valid solution is where the two graphs intersect.",
    x = "x",
    y = "y",
    linetype = "Function"
  ) +
  book_theme()
```

::: {.cell-output-display}
![Solving |x - 4| = 2x + 1 by looking for intersections.](index_files/figure-html/fig-modulus-intersection-1.png){#fig-modulus-intersection width=672}
:::
:::


The graph helps us see why $x=1$ is the only solution.

## Modulus inequalities

A modulus inequality can often be interpreted using distance.

For example,

$$
|x-2|<3
$$

means that the distance between $x$ and $2$ is less than $3$.

So

$$
-3<x-2<3.
$$

Adding $2$ throughout gives

$$
-1<x<5.
$$

Therefore, the solution is

$$
x\in(-1,5).
$$


::: {.cell}

```{.r .cell-code}
df <- data.frame(
  x = seq(-4, 8, length.out = 600)
)

df$y <- abs(df$x - 2)

ggplot(df, aes(x = x, y = y)) +
  geom_line(linewidth = 1.1) +
  geom_hline(yintercept = 3, linetype = "dashed") +
  geom_vline(xintercept = -1, linetype = "dashed") +
  geom_vline(xintercept = 5, linetype = "dashed") +
  annotate("text", x = 2, y = 0.5, label = "solution: -1 < x < 5") +
  labs(
    title = "Graph of y = |x - 2| and y = 3",
    subtitle = "The inequality |x - 2| < 3 holds below the dashed horizontal line.",
    x = "x",
    y = "y"
  ) +
  book_theme()
```

::: {.cell-output-display}
![The solution region for |x - 2| < 3.](index_files/figure-html/fig-modulus-inequality-1.png){#fig-modulus-inequality width=672}
:::
:::


## Polynomial division

Polynomial division allows us to divide one polynomial by another.

For example, suppose

$$
f(x)=x^3-2x^2-5x+6.
$$

If we divide $f(x)$ by $x-1$, we obtain

$$
f(x)=(x-1)(x^2-x-6)+0.
$$

Since the remainder is zero, $x-1$ is a factor of $f(x)$.

The quadratic factor can be factorised:

$$
x^2-x-6=(x-3)(x+2).
$$

Therefore,

$$
f(x)=(x-1)(x-3)(x+2).
$$

So the roots are

$$
x=1,\quad x=3,\quad x=-2.
$$

## The factor theorem

The factor theorem states:

If

$$
f(a)=0,
$$

then

$$
x-a
$$

is a factor of $f(x)$.

For example, let

$$
f(x)=x^3-2x^2-5x+6.
$$

Testing $x=1$ gives

$$
f(1)=1^3-2(1)^2-5(1)+6=0.
$$

Therefore, $x-1$ is a factor of $f(x)$.

## The remainder theorem

The remainder theorem states:

When $f(x)$ is divided by $x-a$, the remainder is $f(a)$.

For example, let

$$
f(x)=x^3-2x^2-5x+6.
$$

If we divide by $x-2$, then the remainder is $f(2)$.

Now,

$$
f(2)=2^3-2(2)^2-5(2)+6.
$$

So

$$
f(2)=8-8-10+6=-4.
$$

Therefore, the remainder is

$$
-4.
$$


::: {.cell}

```{.r .cell-code}
f <- function(x) x^3 - 2*x^2 - 5*x + 6

f(1)
```

::: {.cell-output .cell-output-stdout}

```
[1] 0
```


:::

```{.r .cell-code}
f(2)
```

::: {.cell-output .cell-output-stdout}

```
[1] -4
```


:::
:::


The R output confirms that $f(1)=0$ and $f(2)=-4$.

## Visualisation using R: polynomial roots


::: {.cell}

```{.r .cell-code}
f <- function(x) x^3 - 2*x^2 - 5*x + 6

df <- data.frame(
  x = seq(-3.5, 4, length.out = 800)
)

df$y <- f(df$x)

roots <- data.frame(
  x = c(-2, 1, 3),
  y = c(0, 0, 0)
)

ggplot(df, aes(x = x, y = y)) +
  geom_line(linewidth = 1.1) +
  geom_hline(yintercept = 0) +
  geom_vline(xintercept = 0) +
  geom_point(data = roots, aes(x = x, y = y), size = 3) +
  annotate("text", x = -2, y = 5, label = "x = -2") +
  annotate("text", x = 1, y = 5, label = "x = 1") +
  annotate("text", x = 3, y = 5, label = "x = 3") +
  labs(
    title = expression(f(x) == x^3 - 2*x^2 - 5*x + 6),
    subtitle = "The roots occur where the graph crosses the x-axis.",
    x = "x",
    y = "f(x)"
  ) +
  book_theme()
```

::: {.cell-output-display}
![The graph of f(x) = x^3 - 2x^2 - 5x + 6.](index_files/figure-html/fig-cubic-roots-1.png){#fig-cubic-roots width=672}
:::
:::


The roots of the polynomial are the $x$-values where the graph crosses the $x$-axis.

## Worked example 3: using the factor theorem

Show that $x-2$ is a factor of

$$
f(x)=x^3-5x^2+2x+8.
$$

By the factor theorem, $x-2$ is a factor if

$$
f(2)=0.
$$

Now,

$$
f(2)=2^3-5(2)^2+2(2)+8.
$$

Therefore,

$$
f(2)=8-20+4+8=0.
$$

Hence,

$$
x-2
$$

is a factor of $f(x)$.

We can now factorise fully.

Since $x-2$ is a factor,

$$
f(x)=(x-2)(x^2-3x-4).
$$

Then

$$
x^2-3x-4=(x-4)(x+1).
$$

So

$$
f(x)=(x-2)(x-4)(x+1).
$$

The roots are

$$
x=2,\quad x=4,\quad x=-1.
$$


::: {.cell}

```{.r .cell-code}
f <- function(x) x^3 - 5*x^2 + 2*x + 8

df <- data.frame(
  x = seq(-2, 5, length.out = 800)
)

df$y <- f(df$x)

roots <- data.frame(
  x = c(-1, 2, 4),
  y = c(0, 0, 0)
)

ggplot(df, aes(x = x, y = y)) +
  geom_line(linewidth = 1.1) +
  geom_hline(yintercept = 0) +
  geom_vline(xintercept = 0) +
  geom_point(data = roots, aes(x = x, y = y), size = 3) +
  labs(
    title = expression(f(x) == x^3 - 5*x^2 + 2*x + 8),
    subtitle = "The graph confirms the three real roots.",
    x = "x",
    y = "f(x)"
  ) +
  book_theme()
```

::: {.cell-output-display}
![The graph of f(x) = x^3 - 5x^2 + 2x + 8.](index_files/figure-html/fig-factor-theorem-example-1.png){#fig-factor-theorem-example width=672}
:::
:::


## Short investigation 1: changing a modulus function

Consider the family of functions

$$
y=|x-a|.
$$

Change the value of $a$ in the code below.


::: {.cell}

```{.r .cell-code}
a <- 2

df <- data.frame(
  x = seq(-5, 7, length.out = 600)
)

df$y <- abs(df$x - a)

ggplot(df, aes(x = x, y = y)) +
  geom_line(linewidth = 1.1) +
  geom_hline(yintercept = 0) +
  geom_vline(xintercept = 0) +
  geom_vline(xintercept = a, linetype = "dashed") +
  labs(
    title = paste0("Graph of y = |x - ", a, "|"),
    subtitle = "Change a and observe how the turning point moves.",
    x = "x",
    y = "y"
  ) +
  book_theme()
```

::: {.cell-output-display}
![Exploring the family of graphs y = |x - a|.](index_files/figure-html/fig-investigation-modulus-family-1.png){#fig-investigation-modulus-family width=672}
:::
:::


Questions to explore:

1. What happens when $a$ increases?
2. What happens when $a$ decreases?
3. Where is the turning point of $y=|x-a|$?
4. How would you sketch $y=|x+3|$ without using R?

## Short investigation 2: changing the roots of a cubic

Consider the polynomial

$$
f(x)=(x-a)(x-b)(x-c).
$$

Change the values of $a$, $b$, and $c$.


::: {.cell}

```{.r .cell-code}
a <- -2
b <- 1
c <- 3

f <- function(x) (x - a) * (x - b) * (x - c)

df <- data.frame(
  x = seq(-5, 5, length.out = 800)
)

df$y <- f(df$x)

root_df <- data.frame(
  x = c(a, b, c),
  y = c(0, 0, 0)
)

ggplot(df, aes(x = x, y = y)) +
  geom_line(linewidth = 1.1) +
  geom_hline(yintercept = 0) +
  geom_vline(xintercept = 0) +
  geom_point(data = root_df, aes(x = x, y = y), size = 3) +
  labs(
    title = "Graph of f(x) = (x - a)(x - b)(x - c)",
    subtitle = "The roots are controlled by a, b and c.",
    x = "x",
    y = "f(x)"
  ) +
  book_theme()
```

::: {.cell-output-display}
![Exploring how the roots of a cubic affect its graph.](index_files/figure-html/fig-investigation-cubic-roots-1.png){#fig-investigation-cubic-roots width=672}
:::
:::


Questions to explore:

1. What happens if two roots are equal?
2. What happens if all three roots are positive?
3. What happens if one root is negative and two are positive?
4. How does the graph behave for very large positive or negative values of $x$?

## Exam-style practice

These questions are original practice questions written to match the style of the topic.

### Question 1

Solve

$$
|3x-2|=7.
$$

### Question 2

Solve

$$
|x+5|=2x-1.
$$

Remember to check any candidate solutions.

### Question 3

Solve the inequality

$$
|x-4|\leq 6.
$$

Give your answer as an interval.

### Question 4

Let

$$
f(x)=x^3-4x^2-x+4.
$$

Show that $x-1$ is a factor of $f(x)$.

Then factorise $f(x)$ fully.

### Question 5

Let

$$
g(x)=2x^3-3x^2-8x+12.
$$

Find the remainder when $g(x)$ is divided by $x-2$.

### Question 6

The polynomial

$$
p(x)=x^3+kx^2-7x+6
$$

has $x-1$ as a factor.

Find the value of $k$.

### Question 7

A cubic polynomial has roots

$$
x=-1,\quad x=2,\quad x=5.
$$

Write down one possible polynomial with these roots.

Then sketch its graph using R.

### Question 8

The graph of

$$
y=|ax+b|
$$

has turning point at

$$
x=3.
$$

Give one possible pair of values for $a$ and $b$.

Explain your answer.

## Reflection questions

1. Why does $y=|x|$ have a sharp corner at $x=0$?
2. Why must we check solutions when solving equations such as $|x-4|=2x+1$?
3. What is the connection between the factor theorem and the roots of a polynomial?
4. How does the remainder theorem reduce polynomial division to substitution?
5. How can graphs help us understand algebraic solutions?
6. What can R show clearly that is harder to see from algebra alone?
7. What can algebra show exactly that a graph can only suggest approximately?
