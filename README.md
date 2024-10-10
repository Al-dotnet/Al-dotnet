using Microsoft.AspNetCore.Mvc;

namespace PromptApp.Controllers
{
    public class PromptController : Controller
    {
        [HttpGet]
        public IActionResult Index()
        {
            return View();
        }

        [HttpPost]
        public IActionResult Index(string command)
        {
            string result;

            // Process the command input
            switch (command?.ToLower())
            {
                case "greet":
                    result = "Hello, user!";
                    break;
                case "add":
                    result = "Please enter two numbers (e.g., add 3 4)";
                    break;
                case var c when c.StartsWith("add"):
                    var parts = command.Split(' ');
                    if (parts.Length == 3 && double.TryParse(parts[1], out double num1) && double.TryParse(parts[2], out double num2))
                    {
                        result = $"The sum is: {num1 + num2}";
                    }
                    else
                    {
                        result = "Invalid format for add. Example: add 3 4";
                    }
                    break;
                case "exit":
                    result = "Goodbye!";
                    break;
                default:
                    result = "Invalid command. Try 'greet', 'add', or 'exit'.";
                    break;
            }

            // Return the result to the view
            ViewBag.Result = result;
            return View();
        }
    }
}
