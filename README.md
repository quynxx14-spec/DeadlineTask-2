# DeadlineTask-2
DeadlineTask.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DeadlineTask {
    struct Task {
        string title;
        uint256 deadline;
        bool completed;
    }

    Task[] public tasks;

    function createTask(string memory _title, uint256 _days) public {
        tasks.push(Task(_title, block.timestamp + _days * 1 days, false));
    }

    function completeTask(uint256 _index) public {
        require(_index < tasks.length, "Task not found");
        require(block.timestamp <= tasks[_index].deadline, "Deadline passed");
        tasks[_index].completed = true;
    }

    function getOverdueTasks() public view returns (uint256[] memory) {
        uint256 count = 0;
        for (uint i = 0; i < tasks.length; i++) {
            if (!tasks[i].completed && block.timestamp > tasks[i].deadline) count++;
        }
        
        uint256[] memory overdue = new uint256[](count);
        uint256 index = 0;
        for (uint i = 0; i < tasks.length; i++) {
            if (!tasks[i].completed && block.timestamp > tasks[i].deadline) {
                overdue[index] = i;
                index++;
            }
        }
        return overdue;
    }
}
