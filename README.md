# NAME

DBIx::Class::InflateColumn::Geo - Inflate geometric columns to data structures

<div>
    <p>
    <img src="https://img.shields.io/badge/perl-5.10.1+-blue.svg" alt="Requires Perl 5.10.1+" />
    <a href="https://travis-ci.org/Csson/p5-DBIx-Class-InflateColumn-Geo"><img src="https://api.travis-ci.org/Csson/p5-DBIx-Class-InflateColumn-Geo.svg?branch=master" alt="Travis status" /></a>
    <a href="http://cpants.cpanauthors.org/release/CSSON/DBIx-Class-InflateColumn-Geo-0.0100"><img src="http://badgedepot.code301.com/badge/kwalitee/CSSON/DBIx-Class-InflateColumn-Geo/0.0100" alt="Distribution kwalitee" /></a>
    <a href="http://matrix.cpantesters.org/?dist=DBIx-Class-InflateColumn-Geo%200.0100"><img src="http://badgedepot.code301.com/badge/cpantesters/DBIx-Class-InflateColumn-Geo/0.0100" alt="CPAN Testers result" /></a>
    </p>
</div>

# VERSION

Version 0.0100, released 2018-10-27.

# SYNOPSIS

    package TheSchema::Result::Park;
    use base 'DBIx::Class::Core';

    __PACKAGE__->load_components(qw/InflateColumn::Geo/);
    __PACKAGE__->add_columns({
        location => {
            data_type => 'point'
        },
        secondary_location => {
            data_type => 'point',
            geo_srid => 4326,
            geo_xname => 'longitude',
            geo_yname => 'latitude',
            geo_with_astext => 1,
        },
    });

Later:
    my $park = $schema->resultset('Park')->create({
        location => { x => 15.43, y => 54.32 },
        secondary\_location => { longitude => 12.32, latitude => 45.9843 },
    });
    say $park->location->{'x'};                  # 15.43
    say $park->secondary\_location->{'latitude'}; # 45.9843
    say $park->secondary\_location->{'astext'};   # POINT(12.32 45.9843)

# DESCRIPTION

DBIx::Class::InflateColumn::Geo inflates geometry columns (so far, only `POINT` is supported) to more accessible data structures.

# COLUMN SPECIFICATION OPTIONS

Usage shown in the synopsis.

## geo\_srid

The spacial reference identifier you wish to use.

Default: `4326` (aka [WGS 84](http://spatialreference.org/ref/epsg/wgs-84/))

Set it to `undef` if you want to use your database's default.

## geo\_xname

The name you wish to use for the `X` (or `longitude`) value.

Default: `x`

## geo\_yname

The name you wish to use for the `Y` (or `latitude`) value.

Default: `y`

## geo\_with\_astext

A boolean determining whether the [Well-known text](https://en.wikipedia.org/wiki/Well-known_text) of the column is included in the inflated hash. It is not used during deflation.

Default: `0`

# COMPATIBILITY

I have only tested this on MariaDB 10.\*.

# SEE ALSO

- [DBIx::Class::GeomColumns](https://metacpan.org/pod/DBIx::Class::GeomColumns)

# SOURCE

[https://github.com/Csson/p5-DBIx-Class-InflateColumn-Geo](https://github.com/Csson/p5-DBIx-Class-InflateColumn-Geo)

# HOMEPAGE

[https://metacpan.org/release/DBIx-Class-InflateColumn-Geo](https://metacpan.org/release/DBIx-Class-InflateColumn-Geo)

# AUTHOR

Erik Carlsson <info@code301.com>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2018 by Erik Carlsson.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
