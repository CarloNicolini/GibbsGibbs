cmake_minimum_required (VERSION 2.6)

set(STATISTIC_HDRS 
#BayesNetwork.h
Density.h
DExample.h
Distribution.h
DMultivariateGaussian.h
Inferencer.h
Sampler.h
)

set(STATISTIC_SRCS
#BayesNetwork.cpp
Density.cpp
DExample.cpp
DMultivariateGaussian.cpp
Inferencer.cpp
Sampler.cpp
)

add_library(GibbsGibbs ${STATISTIC_HDRS} ${STATISTIC_SRCS} )

add_executable(testStatistic01 testStatistic01.cpp)

target_link_libraries(testStatistic01 GibbsGibbs)
