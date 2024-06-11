# appendObject
javascript code to append a property to an object. If the property exists do nothing. Creating this method cause I was tired of doing a bunch of checks to make before adding a property to an object. 

Here is the code. Its only a few liens so easy to copy and paste. 

addPropertyIfNotExists(newConfig, originalConfig) {
  for (let key in newConfig) {
    if (originalConfig[key]) {  // check if a property already exists. If it does we need to check if it has nested objects. 
      if (typeof newConfig[key] === 'object' && !Array.isArray(newConfig[key] && newConfig[key] !== null) {
        // if the value of the key is a object then call the same function recursively to drill down into the nested objects
        addPropertyIfNotExists(newConfig[key], originalConfig[key]);
      } 
    } else if (originalConfig[key] !== false) { // if the original config has the property set to false then we can skip, we need to only add the property if it does not exists
      originalConfig[key] = newConfig[key];  // once we reach here, we know that the property does not exists, so we add it in. 
    }
  }
}

Example Usage 

Example 1: 
const newConfig = {
  options: {
    scales: {
      x: {
        grid: {
          display: false // want to set this to the origin config. 
        }
      }
    }
  }
};

const originalConfig = {
  type: 'line',
  data: this.data,
  options: {
    parsing: {
      xAxisKey: 'year',
      yAxisKey: 'sale'
    },
    scales: {
      y: {
        border: {
          display: false
        }
      }
    }
  }
};

Output after calling the function 

addPropertyIfNotExists(newConfig, originalConfig);

/****Output ******
{
  type: 'line',
  data: this.data,
  options: {
    parsing: {
      xAxisKey: 'year',
      yAxisKey: 'sale'
    },
    scales: {
      x: {
        grid: {
          display: false // want to set this to the origin config. 
        }
      },
      y: {
        border: {
          display: false
        }
      }
    }
  }
};
******************/

