---
layout: post
title:      "React: Building Chart Components"
date:       2018-09-17 14:10:02 -0400
permalink:  react_building_chart_components
---


My React final project is an application where you can record all of the Fortnite games you have played. A user can save all of the relevant data such as the game mode, the total number of enemy eliminations per game, the user's final standing, and any comments on that particular game.  I quickly realized that an important facet of this application would be the ability to review this data in aggregate using charts.    

Initially, I used a Sparklines charting component. The documentation for Sparklines can be found [here](https://www.npmjs.com/package/react-sparklines).  Sparklines can be used to create line and bar charts. I installed the Sparklines library by running :
```
npm install react-sparklines --save
```
and importing it into my chart component like this: 
```
import { Sparklines, SparklinesLine, SparklinesReferenceLine } from 'react-sparklines';
```

The reusable chart component I created simply returned a Sparkline component:

```
const Chart = (props) => {
 
 return(
     <div>
       <Sparklines height={120} width={180} data={props.data}>
         <SparklinesLine  color={props.color} />
       </Sparklines>
     </div>
		 
}		 
```

I then created several charts by passing each individual charts data as props. This is how they looked initially:

![](https://i.imgur.com/rdu8Ylz.png)

If I wanted to add any additional styling or configuration to the chart it would have to be done with HTML and CSS.  Also, I decided that some data would benefit from being shown in a different format, not a line or bar chart.  What would happen if I wanted the data to be presented in a pie chart or a scatter plot? These aren't options when using Sparklines. So, because of these limitations I decided to find a more robust charting system.

That is exactly what I found with [react-chartjs-2](https://github.com/jerairrest/react-chartjs-2) .
With react-chartjs-2 you can import various different types of charts into your application and configure them in many different ways. For example, I decided that a user of my application might want to view all of the games that he or she had played altogether to see which game mode they played the most.  To do this I created a pie chart and pased in all the data as props:

![](https://i.imgur.com/qFyxKYC.png)

This was created simply by changing a few options in the chart component. There was no need to manipulate the HTML or CSS to get this optput. Since the data comes from the props the chart updates dynamically as new games are added to the database. 

Also, when installing the react-chartjs-2 package all chart options are available - it is simply a matter of importing the type of chart you want into your component and changing the name of the component to reflect that chart type.  For example, the code below shows how I created a pie chart.  To change that to a bar chart I would only have to change the import from 'Pie' to 'Bar' and change the component name from 'Pie' to 'Bar'. The chart would automatically re-render as a bar chart.

```
import React from 'react'
import {Pie} from 'react-chartjs-2';


export default (props) => {

    const chartData = {
      labels:  ['Solo', 'Squad', '50v50', 'Duo', 'Playground'],
      datasets: [{
        // label: 'Game Play',
        data: props.data,
        backgroundColor: ['red', 'orange', 'green', 'purple', 'blue']
      }],
      borderWidth: 1,
      borderColor: 'gray',
      hoverBorderWidth: 3,
      hoverBorderColor: 'black'
    }


  return(
    <div className="chart">
      <Pie
        data={chartData}
        width={450}  height={350}
        options={{
          layout:{
            padding:{
              left: 50
            }
          },
          toolTips:{
            enabled: true
          },

          maintainAspectRatio: false,
            title: {
              display: true,
              text: 'Total Games Played by Game Mode',
              fontSize: 20
              },
            legend: {
              display: true,
              position: 'bottom',
              labels:{
                fontColor: 'black'
              }
              }
        }}
      />

    </div>
  )
}

```

Installing 3rd party components to your react app can greatly improve its functionality and user experience. There are many different options available to achieve your results.  Personally, I'm looking forward to diving into [react D3](http://www.reactd3.org/) as the next step on my charting journey.


