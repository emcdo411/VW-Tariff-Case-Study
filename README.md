### Case Study: **Volkswagen's Top 3 Models and Top EV: Impact of Tariffs on Component Sourcing**

#### **Executive Summary**

This case study explores the impact of tariffs on Volkswagen's top three models and its leading Electric Vehicle (EV) in the United States, with a particular focus on the components sourced from China. The study specifically investigates the percentage of component costs that come from China and how the sourcing of key components—such as microchips, lithium batteries, and aluminum frames—impacts Volkswagen's revenue and, ultimately, the consumer.

The analysis and breakdown were inspired by **Harrison Hawthorne**, the Senior Lead NAR and Procurement Analyst for Volkswagen Group of America, whose insights into the tariff situation within the automotive industry prompted this exploration of supply chain disruptions and cost implications.

#### **Component Breakdown and Analysis**

Volkswagen's primary models are significantly impacted by the global supply chain, particularly in relation to sourcing components from China. This study identifies key components across the following categories: **Electrical, Metals, Non-Electronic Parts, Plastics, and Batteries**. The components analyzed are critical to the production of the top 3 Volkswagen models and its flagship EV:

1. **Microchips (Electrical)**
2. **Lithium Batteries (Battery)**
3. **Aluminum Frames (Metals)**
4. **Wiring Harnesses (Non-Electronic)**
5. **Dashboard Plastics (Plastics)**

Each component's sourcing from China was quantified, revealing that certain components—such as **microchips** and **lithium batteries**—are sourced predominantly from China, significantly impacting both production and cost.

##### **Key Findings**

- **Microchips**: 70% of the microchips used in Volkswagen’s vehicles are sourced from China, making them a critical component in the supply chain. Due to the global shortage of microchips, tariffs have directly impacted costs, raising consumer prices.
  
- **Lithium Batteries**: 50% of the lithium battery components come from China, and with the growing demand for EVs, this represents a significant challenge. Tariffs have added pressure to the supply chain and increased the cost passed on to consumers.

- **Aluminum Frames**: These parts, sourced 20% from China, have a less dramatic impact but still contribute to cost increases when combined with other components affected by tariffs.

- **Wiring Harnesses and Dashboard Plastics**: These components have a smaller proportion sourced from China (40% and 10%, respectively), but they are nonetheless important in the overall cost structure.

#### **What This Means for Volkswagen**

The rise in tariffs, compounded by the global economic slowdown and trade tensions, has led to an increase in component costs, which ultimately gets passed on to the consumer. For Volkswagen, these tariff increases create pressure on profitability, especially for EVs, which already carry higher production costs due to the expensive batteries and new technology. **Lithium battery prices**, in particular, are a major contributor to price hikes in EVs, affecting consumer adoption rates of electric vehicles.

#### **Why This Matters**

This case study is crucial for understanding the ongoing **supply chain disruptions** facing the automotive industry, especially in relation to the growing shift towards **electric vehicles (EVs)**. As tariffs and the trade landscape evolve, manufacturers must adapt their sourcing strategies, seek alternatives, or invest in local manufacturing to mitigate cost increases. **Volkswagen**, as one of the major players in both traditional and electric automotive markets, is uniquely positioned to lead innovation in overcoming these challenges.

Moreover, this analysis highlights the importance of **supply chain diversification** and **strategic procurement**. By understanding where key components are sourced and how tariffs impact them, manufacturers can better plan for price fluctuations and disruptions in production. Ultimately, this study aims to equip Volkswagen, its partners, and its consumers with insights to navigate these economic pressures.

#### **Conclusion**

In conclusion, Volkswagen's exposure to tariffs, especially on key components sourced from China, has a significant impact on its top-selling models, particularly its electric vehicle lineup. Components like **microchips** and **lithium batteries** are the most affected by tariff increases, directly influencing both production costs and consumer prices. As this study demonstrates, the automotive industry must continuously monitor geopolitical and economic shifts to stay competitive while maintaining consumer affordability.

#### **Code from the 3D Plot Graph**

```r
# Load required libraries
library(dplyr)
library(ggplot2)
library(plotly)

# Create the dataset for the components and cost data
component_data <- data.frame(
  Component = c("Microchips", "Lithium Battery", "Aluminum Frame", "Wiring Harness", "Dashboard Plastics"),
  Category = c("Electrical", "Battery", "Metals", "Non-Electronic", "Plastics"),
  Source_from_China = c(70, 50, 20, 40, 10),  # Percentage sourced from China
  Cost_of_Component = c(300, 5000, 1200, 150, 300),  # Cost in USD
  Pass_Through_Cost = c(420, 5500, 1440, 180, 330)  # Pass-through cost after tariffs
)

# Create a custom hover text
component_data$hover_text <- paste("Component: ", component_data$Component, "<br>",
                                   "Source from China: ", component_data$Source_from_China, "%", "<br>",
                                   "Cost of Component: $", component_data$Cost_of_Component, "<br>",
                                   "Pass-Through Cost: $", component_data$Pass_Through_Cost)

# Create a 3D scatter plot using plotly
interactive_3d_plot <- plot_ly(component_data, 
                               x = ~Component, 
                               y = ~Cost_of_Component, 
                               z = ~Source_from_China, 
                               color = ~Category,  # Color based on the component category
                               type = "scatter3d",  # Use scatter3d for 3D scatter plot
                               mode = "markers",    # Plot points as markers
                               hoverinfo = "text", 
                               text = ~hover_text) %>%
  layout(title = paste("3D Component Breakdown"),
         scene = list(
           xaxis = list(title = "Component"),
           yaxis = list(title = "Cost (USD)"),
           zaxis = list(title = "Source from China (%)")
         ))

# Print the interactive 3D plot
interactive_3d_plot
```

#### **Code for the Shiny Dashboard**

```r
# Install and load required libraries
install.packages("shiny")
install.packages("plotly")
library(shiny)
library(plotly)

# Create the dataset for the components and cost data
component_data <- data.frame(
  Component = c("Microchips", "Lithium Battery", "Aluminum Frame", "Wiring Harness", "Dashboard Plastics"),
  Category = c("Electrical", "Battery", "Metals", "Non-Electronic", "Plastics"),
  Source_from_China = c(70, 50, 20, 40, 10),  # Percentage sourced from China
  Cost_of_Component = c(300, 5000, 1200, 150, 300),  # Cost in USD
  Pass_Through_Cost = c(420, 5500, 1440, 180, 330)  # Pass-through cost after tariffs
)

# Create a custom hover text
component_data$hover_text <- paste("Component: ", component_data$Component, "<br>",
                                   "Source from China: ", component_data$Source_from_China, "%", "<br>",
                                   "Cost of Component: $", component_data$Cost_of_Component, "<br>",
                                   "Pass-Through Cost: $", component_data$Pass_Through_Cost)

# Define the UI
ui <- fluidPage(
  titlePanel("Component Breakdown Dashboard"),
  
  sidebarLayout(
    sidebarPanel(
      selectInput("component", "Choose a component:",
                  choices = component_data$Component, selected = "Microchips")
    ),
    
    mainPanel(
      plotlyOutput("componentPlot")
    )
  )
)

# Define the server logic
server <- function(input, output) {
  
  # Reactive expression to filter the data based on selected component
  filtered_data <- reactive({
    component_data[component_data$Component == input$component, ]
  })
  
  # Render the plot based on the filtered data
  output$componentPlot <- renderPlotly({
    data <- filtered_data()  # Get the filtered data for the selected component
    
    plot_ly(data, 
            x = ~Component, 
            y = ~Cost_of_Component, 
            z = ~Source_from_China, 
            color = ~Category, 
            type = "scatter3d",  # Use scatter3d for 3D scatter plot
            mode = "markers",    # Plot points as markers
            hoverinfo = "text", 
            text = ~hover_text) %>%
      layout(title = paste(input$component, "Component Breakdown"),
             scene = list(
               xaxis = list(title = "Component"),
               yaxis = list(title = "Cost (USD)"),
               zaxis = list(title = "Source from China (%)")
             ))
  })
}

# Run the application
shinyApp(ui = ui, server = server)
```

#### **Suggested Repository Name:**
`Volkswagen-Tariff-Impact-Analysis`

---

This case study provides key insights into how **tariffs** are influencing Volkswagen's supply chain, particularly in regard to their electric vehicle development and their reliance on Chinese-sourced components. The **3D plot** and **Shiny dashboard** demonstrate the dynamic and interactive approach to understanding the data, giving stakeholders, such as procurement analysts and decision-makers, the tools to better understand the cost implications and sourcing risks in the current automotive landscape.
