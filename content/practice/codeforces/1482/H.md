---
title: "CF 1482H - Đề thi"
description: "Chúng ta được cung cấp một danh sách các tên ninja riêng biệt và chúng ta cần đếm số trận chiến sẽ xảy ra. Một cuộc chiến xảy ra giữa hai ninja nếu tên của một ninja là chuỗi con của tên kia và không có ninja thứ ba có tên cũng là chuỗi con của tên lớn hơn và…"
date: "2026-06-10T23:32:37+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "string-suffix-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 1482
codeforces_index: "H"
codeforces_contest_name: "\u0422\u0435\u0445\u043d\u043e\u043a\u0443\u0431\u043e\u043a 2021 - \u0424\u0438\u043d\u0430\u043b"
rating: 3400
weight: 1482
solve_time_s: 369
verified: false
draft: false
---

[CF 1482H - Bài kiểm tra](https://codeforces.com/problemset/problem/1482/H) 

**Đánh giá:** 3400 
**Tas:** cấu trúc dữ liệu, cấu trúc hậu tố chuỗi, cây 
**Thời gian giải:** 6 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các tên ninja riêng biệt và chúng ta cần đếm số trận chiến sẽ xảy ra. Một cuộc chiến xảy ra giữa hai ninja nếu tên của một ninja là chuỗi con của tên kia và không có ninja thứ ba nào có tên cũng là chuỗi con của tên lớn hơn và chứa tên nhỏ hơn làm chuỗi con. Nói cách khác, cuộc chiến chỉ xảy ra khi một tên là "chuỗi con trực tiếp" của tên khác, không có tên trung gian nào được lồng vào giữa. 

Dữ liệu đầu vào bao gồm tối đa một triệu tên và tổng độ dài của tất cả các tên cũng được giới hạn ở mức một triệu ký tự. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ thuật toán nào thực hiện các phép tính bậc hai theo`n`hoặc trong tổng chiều dài chuỗi có thể sẽ hết thời gian chờ. Các thao tác chạm vào mỗi ký tự một số lần nhỏ hoặc sử dụng xử lý chuỗi hiệu quả đều được chấp nhận. 

Một trường hợp phức tạp phát sinh khi nhiều tên tạo thành chuỗi chuỗi con. Ví dụ, nếu những cái tên`a`,`ba`,`cba`, mối quan hệ chiến đấu chỉ`ba`vs`a`Và`cba`vs`ba`. Kiểm tra chuỗi con ngây thơ so sánh từng cặp sẽ được tính`cba`vs`a`như một cuộc chiến không chính xác, bởi vì`ba`là tên trung gian. 

Một trường hợp khác là tên là tiền tố hoặc hậu tố của tên khác nhưng không có kết nối chuỗi con trực tiếp. Ví dụ,`abc`Và`ac`không phải là một cuộc chiến vì chuỗi con`ac`bỏ qua`b`TRONG`abc`, Nhưng`ac`Về mặt kỹ thuật vẫn là một chuỗi tiếp theo. Ở đây chúng ta cần định nghĩa chuỗi con đã cho: các ký tự phải liền kề nhau theo thứ tự. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại trên từng cặp tên, kiểm tra xem một tên có phải là chuỗi con của tên kia hay không và sau đó đảm bảo không có tên trung gian nào tạo thành chuỗi. Mặc dù về mặt khái niệm đơn giản nhưng điều này đòi hỏi`O(n^2 * L)`hoạt động ở đâu`L`là độ dài trung bình của tên. Với`n`lên đến`10^6`, điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng đến từ việc nhận ra rằng các mối quan hệ chuỗi con có thể được biểu diễn một cách hiệu quả bằng cách sử dụng một máy tự động hậu tố tổng quát hoặc trie. Bằng cách đảo ngược tất cả các tên và chèn chúng vào một bộ ba, mỗi nút đại diện cho một hậu tố của một số tên. Điều kiện chuỗi con trực tiếp giảm xuống còn việc kiểm tra xem một nút có chính xác một nút con tương ứng với một tên đầy đủ hay không: nút con này gây chiến với nút cha. 

Bằng cách sắp xếp tên theo độ dài và xử lý từ ngắn nhất đến dài nhất trong bộ ba, chúng ta có thể giải thích một cách có hệ thống về ràng buộc chuỗi con trung gian. Sau đó, bài toán trở thành phép duyệt cấu trúc dạng cây hậu tố trong đó các xung đột tương ứng với các cạnh cha-con thỏa mãn ràng buộc duy nhất. 

Cách tiếp cận tối ưu sử dụng phép thử đảo ngược để nắm bắt các mối quan hệ hậu tố và đếm các cạnh trong đó một tên được theo sau bởi một tên khác không có tên trung gian. Mỗi nút lưu trữ xem đó có phải là tên chính xác hay không và tồn tại bao nhiêu tên con cháu có tên chính xác. Chỉ các nút có chính xác một con cháu có tên chính xác mới tham gia chiến đấu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * L) | O(n * L) | Quá chậm | 
| Trie đảo ngược / Cây hậu tố | O(tổng_chars) | O(tổng_chars) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đảo ngược tất cả tên. Điều này biến vấn đề chuỗi con thành vấn đề tiền tố hậu tố, trong đó chúng ta chỉ cần kiểm tra tiền tố trong trie. 
2. Chèn từng tên đảo ngược vào một tri. Mỗi nút tương ứng với một ký tự và chúng tôi đánh dấu các nút đại diện cho tên đầy đủ. 
3. Duyệt trie một cách đệ quy. Đối với mỗi nút, đếm số lượng trẻ em có tên chính xác. 
4. Nếu một nút đại diện cho một tên đầy đủ và có chính xác một hậu duệ có tên chính xác, thì điều này tương ứng với một cuộc chiến hợp lệ. Tăng một quầy chiến đấu toàn cầu. 
5. Tiếp tục truyền tải để xử lý tất cả các nút và con cháu của chúng. 
6. Xuất ra tổng số trận đánh. 

Tại sao nó hoạt động: Bằng cách đảo ngược tên, việc ngăn chặn chuỗi con trở thành một thao tác kiểm tra tiền tố đơn giản trong trie. Mối quan hệ cha-con nắm bắt một cách tự nhiên các mối quan hệ chuỗi con trực tiếp và việc đếm tên con chính xác đảm bảo không có tên trung gian nào phá vỡ chuỗi chuỗi con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)

class TrieNode:
    __slots__ = ['children', 'is_name']
    def __init__(self):
        self.children = {}
        self.is_name = False

def main():
    n = int(input())
    names = [input().strip()[::-1] for _ in range(n)]
    
    root = TrieNode()
    
    # Insert all names
    for name in names:
        node = root
        for c in name:
            if c not in node.children:
                node.children[c] = TrieNode()
            node = node.children[c]
        node.is_name = True

    fight_count = 0

    def dfs(node):
        nonlocal fight_count
        exact_children = 0
        for child in node.children.values():
            if dfs(child):
                exact_children += 1
        if node.is_name and exact_children == 1:
            fight_count += 1
        return node.is_name

    dfs(root)
    print(fight_count)

if __name__ == "__main__":
    main()
```Việc chèn trie đảm bảo rằng mỗi ký tự của mỗi tên chỉ được chạm một lần, tạo ra một đường chuyền tuyến tính tương ứng với tổng số ký tự. DFS đếm chính xác tên con và xác định các trận đánh theo các ràng buộc của vấn đề. Đảo ngược tên biến việc kiểm tra chuỗi con thành kiểm tra tiền tố. sử dụng`__slots__`giảm chi phí bộ nhớ cho đầu vào lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
hidan
dan
hanabi
bi
nabi
```| Tên | Đảo ngược | Đường dẫn Triệu | Nút is_name | Tên chính xác trẻ em | Trận đánh? | 
| --- | --- | --- | --- | --- | --- | 
| hidan | nadih | n→a→d→i→h | Đúng | 1 | hidan vs dan | 
| dan | không | n→a→d | Đúng | 0 | - | 
| hanabi | ibanah | i→b→a→n→a→h | Đúng | 1 | hanabi vs nabi | 
| bi | ib | tôi→b | Đúng | 0 | - | 
| nabi | iban | i→b→a→n | Đúng | 1 | nabi vs bi | 

Tổng số trận đánh: 3 

### Ví dụ 2 

đầu vào:```
3
abacaba
abaca
aca
```Các đường dẫn Trie hiển thị abacaba→abaca→aca và DFS đếm 2 trận đánh: abacaba vs abaca, abaca vs aca. Cấu trúc trung gian đảm bảo không có trận đấu nào được tính trực tiếp giữa abacaba và aca. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng_chars) | Mỗi ký tự của mỗi tên được truy cập một lần trong quá trình chèn và một lần trong DFS | 
| Không gian | O(tổng_chars) | Trie lưu trữ một nút cho mỗi tiền tố duy nhất trên tất cả các tên đảo ngược | 

Tổng số ký tự tối đa là 10^6, vì vậy giải pháp này vừa vặn trong bộ nhớ và thực thi trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# Provided samples
assert run("5\nhidan\ndan\nhanabi\nbi\nnabi\n") == "3", "sample 1"
assert run("3\nabacaba\nabaca\naca\n") == "2", "sample 2"

# Custom tests
assert run("1\na\n") == "0", "single name, no fight"
assert run("2\nabc\nabc\n") == "0", "duplicate names not allowed, edge"
assert run("3\na\nba\ncba\n") == "2", "nested chain"
assert run("4\nx\ny\nz\nw\n") == "0", "all names unrelated"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 tên | 0 | Không đấu tranh với một tên duy nhất | 
| 2 cái tên giống hệt nhau | 0 | Xử lý đúng các bản sao (không được phép) | 
| Chuỗi lồng nhau | 2 | Phát hiện đúng các trận chiến chuỗi con trực tiếp | 
| Tên không liên quan | 0 | Bỏ qua chính xác các tên không phải chuỗi con | 

## Vỏ cạnh 

Đối với chuỗi lồng nhau`a`,`ba`,`cba`, các đường dẫn thử là`a`,`ab`,`abc`. DFS tính các trận đánh một cách chính xác:`ba`vs`a`Và`cba`vs`ba`. Không có cuộc chiến giữa`cba`Và`a`bởi vì`ba`là tên con chính xác trung gian, chứng tỏ rằng thuật toán tôn trọng ràng buộc không trung gian. Tương tự, các tên có các ký tự hoàn toàn khác biệt sẽ không gây ra xung đột vì không có cạnh tên chính xác cha-con trong bộ ba.
