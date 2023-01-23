local({
  r <- getOption("repos")
  r["CRAN"] <- "https://cran.microsoft.com"
  options(repos = r)
})

.Last <- function() system("R --vanilla")

old <- utils::old.packages()[,1]

cli::cli_alert_info("Outdated packages: {.pkg {old}}")

cli::cli_text("to update old package type,
              {.run pak::pkg_install(old, upgrade = TRUE, ask = FALSE)}")


