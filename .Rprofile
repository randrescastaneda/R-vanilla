local({
  r <- getOption("repos")
  # r["CRAN"] <- "https://cran.microsoft.com"
  # r["CRAN"] <- "cran.r-project.org/"
  options(repos = r)
})

.Last <- function() system("R --vanilla")

# if (!requireNamespace("fs", quietly = TRUE)) {
#   utils::install.packages("fs")
#   .rs.restartR()
# }
#
# if (!requireNamespace("pak", quietly = TRUE)) {
#   utils::install.packages("pak")
#   .rs.restartR()
# }
#
#
#
# if (requireNamespace("cli", quietly = TRUE)) {
#   old <- utils::old.packages()[,1]
#
#   cli::cli_alert_info("Outdated packages: {.pkg {old}}")
#
#   cli::cli_text("to update old package type,
#               {.run pak::pkg_install(old, upgrade = TRUE, ask = FALSE)}")
#
#
# } else {
#   dd <- sessionInfo()
#   major <- dd$R.version$major
#   libuse <- Sys.getenv("R_LIB_USER") |>
#     gsub("")
#
# }

# setting library paths
# from https://milesmcbain.xyz/posts/hacking-r-library-paths/
set_lib_paths <- function(lib_vec) {

  lib_vec <- normalizePath(lib_vec, mustWork = TRUE)

  shim_fun <- .libPaths
  shim_env <- new.env(parent = environment(shim_fun))
  shim_env$.Library <- character()
  shim_env$.Library.site <- character()

  environment(shim_fun) <- shim_env
  shim_fun(lib_vec)
}

Sys.getenv("R_LIBS_USER") |>
  set_lib_paths()





if (FALSE) {
  libuse <- Sys.getenv("R_LIBS_USER") |>
    fs::path_dir()

  vers <- libuse |>
    fs::dir_ls(type = "dir",
               recurse = FALSE) |>
    fs::path_file()


  old_dir  <- libuse |>
    fs::path(vers[length(vers) - 1])

  # old_dir <- "C:/Users/wb384996/R/win-library/" |>
  #   fs::path(vers[length(vers)])

  old <- old_dir |>
    fs::dir_ls(type = "dir") |>
    fs::path_file()

  new_dir <- libuse |>
    fs::path(vers[length(vers)])

  new <- new_dir |>
    fs::dir_ls(type = "dir") |>
    fs::path_file()

  # get only packages that are not in new
  pkg <- setdiff(old, new)

  ls <- pak::lib_status(lib = old_dir) |>
    {\(.) .[.$package %in% pkg, ]}()

  from_gh <- ls[!is.na(ls$remoteusername),] |>
    {\(.) fs::path(.$remoteusername,  .$remoterepo)}()

  # from CRAN
  from_cran <- ls[ls$repository == "CRAN", "package"]

  # get package to be installed
  pkgs <- c(from_gh, from_cran) |>
    {\(.) .[!is.na(.)]}()

  ps_pi <- purrr::possibly(pak::pak,
                           otherwise = NULL)
  pi    <- purrr::map(pkgs, ps_pi, ask = FALSE)
  names(pi) <- pkgs

  # packages that failed
  pi |>
    purrr::keep(is.null) |>
    names()

}



if (FALSE) {
    old <- utils::old.packages()[,1]

    cli::cli_alert_info("Outdated packages: {.pkg {old}}")

    cli::cli_text("to update old package type,
                {.run pak::pkg_install(old, upgrade = TRUE, ask = FALSE)}")


    ps_pi <- purrr::possibly(pak::pak, otherwise = NULL)
    pi    <- purrr::map(old, ps_pi, ask = FALSE)
    names(pi) <- old

    # packages that failed
    pi |>
      purrr::keep(is.null) |>
      names() |>
      {\(xx) cli::cli_text("Packages that failed:
                           {.pkg {xx}}")}()



}
