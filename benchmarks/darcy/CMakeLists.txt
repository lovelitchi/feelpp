###  TEMPLATE.txt.tpl; coding: utf-8 ---

#  Author(s): Christophe Prud'homme <christophe.prudhomme@feelpp.org>
#       Date: 2011-04-02
#
#  Copyright (C) 2011 Université Joseph Fourier (Grenoble I)
#
# Distributed under the GPL(GNU Public License):
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 675 Mass Ave, Cambridge, MA 02139, USA.
#
#

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
foreach( dim 2 3)
  foreach( orderU 0 1 2 )
    set(modulename "darcy")
    MATH(EXPR orderP ${orderU}+1)
    set(BENCH "${modulename}_${dim}D_R${orderU}P${orderP}" )
    set(targetname feel_bench_${BENCH})
    FILE(WRITE ${CMAKE_CURRENT_BINARY_DIR}/${BENCH}.cpp
      "// this file is auto-generated by Feel++ \n\n
    #include<${modulename}.hpp>\n\n
    int main( int argc, char** argv )
    {
      Feel::Assert::setLog( \"${BENCH}.assert\");
      Feel::Environment env( argc, argv, makeOptions() );
      Darcy<${dim}, ${orderU}, ${orderP}> darcy;
      darcy.convergence();
    }
    \n" )

    feelpp_add_application(${targetname} SRCS ${BENCH}.cpp DEFS FEELPP_${dim}D)
  endforeach() # order
endforeach() # dim


