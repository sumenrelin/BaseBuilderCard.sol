# BaseBuilderCard.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract BaseBuilderCard {
    event CardGenerated(
        address indexed owner,
        string name,
        string message,
        string svg,
        uint256 timestamp
    );

    // 生成一张 Base 风格个人卡片（纯链上 SVG）
    function generateCard(
        string memory _name,
        string memory _message
    ) external returns (string memory) {
        require(bytes(_name).length > 0 && bytes(_name).length <= 30, unicode"名字长度 1-30 字符");
        require(bytes(_message).length > 0 && bytes(_message).length <= 100, unicode"留言长度 1-100 字符（支持中文）");

        // 纯链上 SVG 代码（Base 蓝 + 现代设计）
        string memory svg = string(
            abi.encodePacked(
                '<svg xmlns="http://www.w3.org/2000/svg" width="400" height="220" viewBox="0 0 400 220">',
                '<rect width="400" height="220" rx="20" ry="20" fill="#0052FF"/>',  // Base 主色
                '<rect x="20" y="20" width="360" height="180" rx="15" ry="15" fill="#0A0A0A"/>',
                '<text x="200" y="70" font-family="Arial" font-size="28" font-weight="bold" fill="#FFFFFF" text-anchor="middle">',
                _name,
                '</text>',
                '<text x="200" y="110" font-family="Arial" font-size="18" fill="#00FFAA" text-anchor="middle">Base Builder</text>',
                '<text x="200" y="145" font-family="Arial" font-size="16" fill="#FFFFFF" text-anchor="middle" opacity="0.9">',
                _message,
                '</text>',
                '<text x="30" y="200" font-family="Arial" font-size="12" fill="#FFFFFF" opacity="0.6">',
                unicode'Deployed on Base • ',   // ← 这里已修复
                '</text>',
                '<text x="370" y="200" font-family="Arial" font-size="12" fill="#FFFFFF" opacity="0.6" text-anchor="end">',
                'by ',
                '</text>',
                '</svg>'
            )
        );

        emit CardGenerated(msg.sender, _name, _message, svg, block.timestamp);
        return svg;
    }

    // 可选：查看合约信息
    function getInfo() external pure returns (string memory) {
        return "BaseBuilderCard - Generate your on-chain SVG badge for free!";
    }
}
