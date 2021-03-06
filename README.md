# ParquetFiles

[![Project Status: Active - The project has reached a stable, usable state and is being actively developed.](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active)
[![Build Status](https://travis-ci.org/davidanthoff/ParquetFiles.jl.svg?branch=master)](https://travis-ci.org/davidanthoff/ParquetFiles.jl)
[![Build status](https://ci.appveyor.com/api/projects/status/svgqskv2wul3egrr/branch/master?svg=true)](https://ci.appveyor.com/project/davidanthoff/parquetfiles-jl/branch/master)
[![ParquetFiles](http://pkg.julialang.org/badges/ParquetFiles_0.6.svg)](http://pkg.julialang.org/?pkg=ParquetFiles)
[![codecov.io](http://codecov.io/github/davidanthoff/ParquetFiles.jl/coverage.svg?branch=master)](http://codecov.io/github/davidanthoff/ParquetFiles.jl?branch=master)

## Overview

This package provides load support for [Parquet](https://parquet.apache.org/) files under the
[FileIO.jl](https://github.com/JuliaIO/FileIO.jl) package.

## Installation

Use ``Pkg.add("ParquetFiles")`` in Julia to install ParquetFiles and its dependencies.

## Usage

### Load a Parquet file

To read a Parquet file into a ``DataFrame``, use the following julia code:

````julia
using FileIO, ParquetFiles, DataFrames

df = DataFrame(load("data.parquet"))
````

The call to ``load`` returns a ``struct`` that is an [IterableTable.jl](https://github.com/davidanthoff/IterableTables.jl), so it can be passed to any function that can handle iterable tables, i.e. all the sinks in [IterableTable.jl](https://github.com/davidanthoff/IterableTables.jl). Here are some examples of materializing a Parquet file into data structures that are not a ``DataFrame``:

````julia
using FileIO, ParquetFiles, DataTables, IndexedTables, TimeSeries, Temporal, Gadfly

# Load into a DataTable
dt = DataTable(load("data.parquet"))

# Load into an IndexedTable
it = IndexedTable(load("data.parquet"))

# Load into a TimeArray
ta = TimeArray(load("data.parquet"))

# Load into a TS
ts = TS(load("data.parquet"))

# Plot directly with Gadfly
plot(load("data.parquet"), x=:a, y=:b, Geom.line)
````

### Using the pipe syntax

``load`` also support the pipe syntax. For example, to load a Parquet file into a ``DataFrame``, one can use the following code:

````julia
using FileIO, ParquetFiles, DataFrame

df = load("data.parquet") |> DataFrame
````

The pipe syntax is especially useful when combining it with [Query.jl](https://github.com/davidanthoff/Query.jl) queries, for example one can easily load a Parquet file, pipe it into a query, then pipe it to the ``save`` function to store the results in a new file.
