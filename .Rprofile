local({
  r <- getOption("repos")
  # r["CRAN"] <- "https://cran.microsoft.com"
  r["CRAN"] = "https://cran.rstudio.com/"
  options(repos = r)
})

.Last <- function() system("R --vanilla")

if (!requireNamespace("fs", quietly = TRUE)) {
  utils::install.packages("fs")
  .rs.restartR()
}

if (!requireNamespace("pak", quietly = TRUE)) {
  utils::install.packages("pak", dependencies = TRUE)
  .rs.restartR()
}



if (requireNamespace("cli", quietly = TRUE)) {
  old <- utils::old.packages()[,1]

  cli::cli_alert_info("Outdated packages: {.pkg {old}}")

  cli::cli_text("to update old package type,
              {.run pak::pkg_install(old, upgrade = TRUE, ask = FALSE)}")


} else {
  dd <- sessionInfo()
  major <- dd$R.version$major
  libuse <- Sys.getenv("R_LIBS_USER")

}

if (FALSE) {
  libuse <- Sys.getenv("R_LIBS_USER") |>
    fs::path_dir()

  vers <- libuse |>
    fs::dir_ls(type = "dir",
               recurse = FALSE) |>
    fs::path_file()


  old_dir  <- libuse |>
    fs::path(vers[-2])
  old <- old_dir |>
    fs::dir_ls(type = "dir") |>
    fs::path_file()

  new_dir <- libuse |>
    fs::path(vers[-1])
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
  from_cran <- from_cran[!is.na(from_cran)]
  # get package to be installed
  install.packages(from_cran)


  ps_pi <- purrr::possibly(remotes::install_github, otherwise = NULL)
  pi    <- purrr::map(from_gh, ps_pi)
  names(pi) <- from_gh

  # packages that failed
  pi |>
    purrr::keep(is.null) |>
    names()

}



if (FALSE) {
    old <- utils::old.packages()[,1]

    cli::cli_alert_info("Outdated packages: {.pkg {old}}")

    cli::cli_text("to update old package type,
                {.run pak::pak(old, upgrade = TRUE, ask = FALSE)}")


    ps_pi <- purrr::possibly(pak::pak, otherwise = NULL)
    ps_pi <- purrr::possibly(install.packages, otherwise = NULL)
    pi    <- purrr::map(old, ps_pi, , dependencies = TRUE)
    names(pi) <- old

    # packages that failed
    pi |>
      purrr::keep(is.null) |>
      names() |>
      {\(xx) cli::cli_text("Packages that failed:
                           {.pkg {xx}}")}()



}



if (FALSE) {
  pkgs <- pak::lib_status()$package
  pak::pak(pkgs, upgrade = TRUE)
}



if (FALSE) {
  gca <- function(x, ...) {
    gert::git_commit_all(x, ...)
  }

  gp <- function(x = NULL, ...) {
    gert::git_push(x, ...)
  }

  ga <- function(...) {
    gert::git_add(gert::git_status(...)$file)
  }

  gi <- function() {
    gert::git_info()$upstream
  }
  gs <- function() {
    gert::git_status()
  }

}
