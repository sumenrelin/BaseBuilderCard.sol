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

// Baseスタイルの個人カードを生成（完全オンチェーンSVG）
function generateCard(
    string memory _name,
    string memory _message
) external returns (string memory) {
    require(bytes(_name).length > 0 && bytes(_name).length <= 30, unicode"名前は1〜30文字以内で入力してください");
    require(bytes(_message).length > 0 && bytes(_message).length <= 100, unicode"メッセージは1〜100文字以内で入力してください（日本語対応）");

    // 純粋オンチェーンSVGコード（Baseブルー + モダンデザイン）
    string memory svg = string(
        abi.encodePacked(
            '<svg xmlns="http://www.w3.org/2000/svg" width="400" height="220" viewBox="0 0 400 220">',
            '<rect width="400" height="220" rx="20" ry="20" fill="#0052FF"/>',  // Base メインカラー
            '<rect x="20" y="20" width="360" height="180" rx="15" ry="15" fill="#0A0A0A"/>',
            '<text x="200" y="70" font-family="Arial" font-size="28" font-weight="bold" fill="#FFFFFF" text-anchor="middle">',
            _name,
            '</text>',
            '<text x="200" y="110" font-family="Arial" font-size="18" fill="#00FFAA" text-anchor="middle">Base Builder</text>',
            '<text x="200" y="145" font-family="Arial" font-size="16" fill="#FFFFFF" text-anchor="middle" opacity="0.9">',
            _message,
            '</text>',
            '<text x="30" y="200" font-family="Arial" font-size="12" fill="#FFFFFF" opacity="0.6">',
            unicode'Deployed on Base • ',
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

// オプション：コントラクト情報を確認
function getInfo() external pure returns (string memory) {
    return "BaseBuilderCard - オンチェーンSVGバッジを無料で生成できます！";
}
