library(readr)
library(dplyr)
library(tidyr)
library(ggplot2)
library(forcats)
library(ggnewscale)
library(patchwork)
library(svglite)

# ---- Read data ----
infile <- "C:\\Users\\s2673271\\OneDrive - University of Edinburgh\\PhD\\Y1\\Sciaridae\\GRC_expression_analysis\\B_coprophila\\03_BLAST_expressed_genes\\GRC_BLAST_table.tsv"
df <- read_tsv(infile)

# ---- Clean and prep ----
df <- df %>%
  mutate(
    `%Identity` = replace_na(`%Identity`, 0),
    Coverage = replace_na(Coverage, 0)
  ) %>%
  separate(`mean_germ_TPM/mean_soma_TPM`,
           into = c("mean_germ_TPM", "mean_soma_TPM"),
           sep = "_", convert = TRUE) %>%
  mutate(
    development_stage = factor(
      development_stage,
      levels = c("0-4h", "4-8h", "late-larva-early-pupa", "adult")
    )
  )

# ---- Summarize TPMs and alignment score ----
df_summary_tpms <- df %>%
  group_by(gene_id, development_stage) %>%
  summarise(
    mean_germ_TPM = max(mean_germ_TPM, na.rm = TRUE),
    mean_soma_TPM = max(mean_soma_TPM, na.rm = TRUE),
    alignment_score = max((Coverage * `%Identity`) / 100, na.rm = TRUE),
    .groups = "drop"
  ) %>%
  mutate(alignment_score = ifelse(is.infinite(alignment_score) | is.na(alignment_score), 0, alignment_score))

# ---- Long format ----
df_tpms_long <- df_summary_tpms %>%
  pivot_longer(cols = c("mean_germ_TPM", "mean_soma_TPM"),
               names_to = "library_type", values_to = "mean_TPM_raw") %>%
  mutate(
    log_TPM = log1p(mean_TPM_raw),
    sample_type = case_when(
      development_stage %in% c("0-4h", "4-8h") ~ "pre-GRC-elimination",
      library_type == "mean_germ_TPM" ~ "germline_library",
      library_type == "mean_soma_TPM" ~ "somatic_library"
    ),
    gene_id = fct_reorder(gene_id, alignment_score)
  )

# ---- Sex-expression ----
df_summary_sex <- df %>%
  group_by(gene_id, development_stage) %>%
  summarise(sex = paste(unique(sex), collapse = "/"), .groups = "drop")

# ---- Color palettes ----
sex_colors <- c(
  "male" = "#1E90FF",
  "female" = "#FF69B4",
  "both-sexes" = "grey75"
)

library_colours = c(
  "pre-GRC-elimination" = "skyblue",
  "germline_library" = "#6A5ACD",
  "somatic_library" = "#F4A261"
)

# ---- Function to make per-stage plot ----
make_stage_plot <- function(stage, point_size = 4, show_x = TRUE, show_y = TRUE) {
  df_stage_tpms <- df_tpms_long %>% filter(development_stage == stage)
  df_stage_sex <- df_summary_sex %>% filter(development_stage == stage)
  df_stage_align <- df_summary_tpms %>% filter(development_stage == stage)
  
  p <- ggplot(df_stage_tpms, aes(x = log_TPM, y = gene_id, fill = sample_type)) +
    geom_col(position = position_dodge(width = 0.8), width = 0.7) +
    scale_fill_manual(values = library_colours, name = NULL) +
    labs(
      x = if (show_x) "log(TPM + 1)" else NULL,
      y = if (show_y) "Gene ID" else NULL,
      title = stage
    ) +
    theme_minimal(base_size = 12) +
    theme(
      plot.title = element_text(hjust = 0.5, face = "bold"),
      axis.text.y = element_text(size = 10, face="bold"),
      axis.title.x = element_text(size = 12),
      axis.title.y = element_text(size = 12),
      axis.text.x = element_text(size = 12),
      legend.position = "bottom",
      axis.ticks = element_blank()
    ) +
    new_scale_color() +
    geom_point(
      data = df_stage_sex %>% mutate(point_x = 5),
      aes(x = point_x, y = gene_id, color = sex),
      size = point_size, inherit.aes = FALSE
    ) +
    scale_color_manual(values = sex_colors, name = NULL) +
    new_scale_fill() +
    geom_tile(
      data = df_stage_align,
      aes(x = 4.5, y = gene_id, fill = alignment_score),
      width = 0.3, height = 0.8, inherit.aes = FALSE
    ) +
    scale_fill_viridis_c(option = "magma", name = NULL, na.value = "white")
  
  # Remove axis elements if not shown
  if (!show_x) {
    p <- p + theme(axis.text.x = element_blank(),
                   axis.title.x = element_blank())
  }
  if (!show_y) {
    p <- p + theme(axis.title.y = element_blank())
  }
  p
}

# ---- Create per-stage plots ----
p1 <- make_stage_plot("0-4h", point_size = 5, show_x = FALSE, show_y = TRUE)
p2 <- make_stage_plot("4-8h", point_size = 5, show_x = FALSE, show_y = FALSE)
p3 <- make_stage_plot("late-larva-early-pupa", point_size = 4, show_x = TRUE, show_y = TRUE)
p4 <- make_stage_plot("adult", point_size = 3, show_x = TRUE, show_y = FALSE)

# ---- Combine 2×2 grid with shared legend ----
final_plot <- (
  (p1 | p2) /
    (p3 | p4)
) +
  plot_layout(guides = "collect") &
  theme(
    legend.position = "right",
    legend.text = element_text(size = 14),
    legend.key.size = unit(1.0, "lines")
  )

# ---- Show ----
final_plot

ggsave("C:\\Users\\s2673271\\OneDrive - University of Edinburgh\\PhD\\Y1\\Sciaridae\\Paper_GRC_transcription\\Figures\\TPM_mismap_raw.svg", 
       plot = final_plot, width = 15, height = 10, units = "in", dpi = 300)
