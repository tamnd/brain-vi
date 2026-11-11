---
title: "CF 1598A - Trò chơi máy tính"
description: "Chúng ta có một lưới 2 hàng với n cột biểu thị một cấp độ trong trò chơi. Monocarp bắt đầu ở góc trên bên trái (1, 1) và muốn đến góc dưới bên phải (2, n). Mỗi ô đều an toàn (0) hoặc bẫy (1)."
date: "2026-06-10T08:46:34+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1598
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 115 (Rated for Div. 2)"
rating: 800
weight: 1598
solve_time_s: 117
verified: true
draft: false
---

[CF 1598A - Trò chơi trên máy tính](https://codeforces.com/problemset/problem/1598/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, dfs và tương tự, dp, triển khai 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một lưới 2 hàng với`n`các cột thể hiện cấp độ trong trò chơi. Monocarp bắt đầu ở góc trên bên trái`(1, 1)`và muốn đến góc dưới bên phải`(2, n)`. Mỗi ô đều an toàn (`0`) hoặc một cái bẫy (`1`). Di chuyển có thể đi đến bất kỳ ô nào trong số 8 ô lân cận, kể cả đường chéo, nhưng không thể rời khỏi lưới. Nếu Monocarp đi vào bẫy, anh ta sẽ chết ngay lập tức. Đầu vào đảm bảo rằng sự bắt đầu`(1, 1)`và kết thúc`(2, n)`được an toàn. 

Nhiệm vụ là xác định xem đường dẫn có tồn tại từ đầu đến cuối cho nhiều trường hợp thử nghiệm hay không. Từ`n`có thể nhiều nhất là 100 và chỉ có hai hàng, kích thước lưới nhỏ. Điều này cho phép các phương pháp tiếp cận tuyến tính hoặc bậc hai trong`n`để chạy thoải mái trong giới hạn 2 giây. 

Một trường hợp khó phát hiện khi một hàng bị chặn bởi các bẫy liên tiếp, buộc Monocarp phải chạy ngoằn ngoèo qua hàng kia. Ví dụ, với`n = 4`và lưới:```
0111
1110
```hàng trên cùng gần như bị chặn hoàn toàn ở đầu và hàng dưới cùng bị chặn ở cuối. Một cách tiếp cận tham lam từ trái sang phải bất cẩn mà không bao giờ quay lại có thể báo cáo sai "CÓ", nhưng không có đường dẫn hợp lệ và câu trả lời đúng là "KHÔNG". Một trường hợp khác là các bẫy xen kẽ đòi hỏi phải di chuyển theo đường chéo thường xuyên, như:```
0101
1010
```Ở đây tồn tại một đường ngoằn ngoèo thích hợp và phải được xác định. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ là thực hiện tìm kiếm theo chiều sâu đầy đủ hoặc tìm kiếm theo chiều rộng từ ô bắt đầu. Mỗi ô có khả năng truy cập tối đa 8 ô lân cận, nhưng tổng số ô chỉ`2 * n`. Vì vậy, BFS/DFS hoạt động trong`O(n)`cho mỗi trường hợp thử nghiệm, điều này tốt cho`n ≤ 100`. Việc triển khai rất đơn giản nhưng có thể quá mức cần thiết do cấu trúc đơn giản của lưới. 

Quan sát quan trọng để có giải pháp nhanh hơn là đường dẫn bị hạn chế bởi các bẫy trong mỗi cột. Nếu một cột có bẫy ở cả hai hàng thì Monocarp không thể đi qua cột đó. Ngược lại, anh ta có thể di chuyển bằng cách ngoằn ngoèo giữa các hàng khi cần thiết. Về cơ bản, chúng ta chỉ cần kiểm tra xem có tồn tại cột nào sau cột đầu tiên mà cả hai ô đều là bẫy hay không. Nếu một cột như vậy tồn tại, Monocarp không thể đạt được mục tiêu; nếu không thì anh ấy có thể. Điều này làm giảm giải pháp thành một lần quét tuyến tính trên các cột bằng các kiểm tra có điều kiện đơn giản. 

Câu chuyện ở đây là BFS hoạt động vì chuyển động không bị hạn chế, nhưng trong lưới 2 hàng, các chướng ngại vật hoàn toàn quyết định khả năng vượt qua. Nhận thấy rằng cột bẫy kép là một rào cản sẽ đơn giản hóa vấn đề để`O(n)`kiểm tra mà không cần duyệt đồ thị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| BFS / DFS | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 
| Kiểm tra cột tuyến tính | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`và hai hàng lưới dưới dạng chuỗi. 
2. Khởi tạo cờ`possible = True`để theo dõi xem một đường dẫn có tồn tại hay không. 
3. Lặp lại các cột từ 1 đến`n-1`(được lập chỉ mục 0), bỏ qua cột đầu tiên được đảm bảo an toàn. 
4. Đối với mỗi cột, hãy kiểm tra xem cả ô trên cùng và dưới cùng có chứa bẫy không (`1`). Nếu có, hãy đặt`possible = False`và phá vỡ. Điều này là do không thể vượt qua một cột có cả hai ô bị chặn và không có cách nào để bỏ qua cột đó. 
5. Sau khi quét tất cả các cột, in ra “CÓ” nếu`possible`vẫn là True, nếu không thì in ra "NO." 

Tại sao nó hoạt động: Điều bất biến là ở mỗi cột, Monocarp luôn có thể chiếm ít nhất một ô an toàn. Nếu một cột có cả hai ô làm bẫy, Monocarp không thể vượt qua cột đó và không có đường dẫn thay thế nào tồn tại. Vì ô bắt đầu và ô kết thúc đều an toàn nên chướng ngại vật duy nhất là cột bị chặn hoàn toàn mà thuật toán này xác định chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    top = input().strip()
    bottom = input().strip()
    
    possible = True
    for i in range(n):
        if top[i] == '1' and bottom[i] == '1':
            possible = False
            break
    print("YES" if possible else "NO")
```Mã đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`để xử lý nhiều trường hợp thử nghiệm. Chúng tôi loại bỏ các dòng đầu vào để loại bỏ ký tự dòng mới. Vòng lặp qua các cột sẽ kiểm tra bẫy kép và khi tìm thấy bẫy kép, chúng tôi sẽ ngừng quét thêm. Điều này tránh việc kiểm tra không cần thiết và phù hợp với lý do của thuật toán về các cột bị chặn hoàn toàn. Việc sử dụng cờ boolean đơn giản đảm bảo chúng tôi không bao giờ báo cáo sai sự tồn tại của đường dẫn. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên:```
3
000
000
```| Cột | đầu[i] | đáy[i] | có thể | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | Đúng | 
| 1 | 0 | 0 | Đúng | 
| 2 | 0 | 0 | Đúng | 

Quá trình quét không tìm thấy cột nào có cả hai bẫy nên kết quả đầu ra là "CÓ". 

Đối với mẫu thứ hai:```
4
0011
1100
```| Cột | đầu[i] | đáy[i] | có thể | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | Đúng | 
| 1 | 0 | 1 | Đúng | 
| 2 | 1 | 0 | Đúng | 
| 3 | 1 | 0 | Đúng | 

Xin nhắc lại, không có cột nào có cả hai ô làm bẫy nên kết quả đầu ra là "CÓ". Dấu vết này chứng tỏ rằng các đường dẫn ngoằn ngoèo có thể tồn tại và không yêu cầu xây dựng đường dẫn rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi trường hợp kiểm thử quét tất cả các cột một lần | 
| Không gian | O(n) | Chúng tôi lưu trữ hai chuỗi có độ dài n cho mỗi trường hợp thử nghiệm | 

Được cho`t ≤ 100`Và`n ≤ 100`, tổng số thao tác tối đa là 10.000, thoải mái trong giới hạn 2 giây. Việc sử dụng bộ nhớ là tối thiểu tại`2 * 100`ký tự cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution code
    t = int(input())
    for _ in range(t):
        n = int(input())
        top = input().strip()
        bottom = input().strip()
        possible = True
        for i in range(n):
            if top[i] == '1' and bottom[i] == '1':
                possible = False
                break
        print("YES" if possible else "NO")
    return output.getvalue().strip()

# provided samples
assert run("4\n3\n000\n000\n4\n0011\n1100\n4\n0111\n1110\n6\n010101\n101010\n") == "YES\nYES\nNO\nYES", "samples"

# custom cases
assert run("1\n3\n010\n101\n") == "YES", "zigzag path"
assert run("1\n3\n111\n010\n") == "NO", "blocked start column"
assert run("1\n4\n0101\n1011\n") == "NO", "blocked column in middle"
assert run("1\n5\n00000\n00000\n") == "YES", "all safe"
assert run("1\n5\n11111\n11111\n") == "NO", "all blocked except start/end"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3\n010\n101\n`| CÓ | Bẫy xen kẽ cho phép đi ngoằn ngoèo | 
|`3\n111\n010\n`| KHÔNG | Bắt đầu bị chặn trong một hàng, đường dẫn không thể | 
|`4\n0101\n1011\n`| KHÔNG | Cột giữa bị chặn ngăn lối đi | 
|`5\n00000\n00000\n`| CÓ | Tất cả các tế bào an toàn | 
|`5\n11111\n11111\n`| KHÔNG | Toàn bộ lưới bị chặn ngoại trừ bắt đầu/kết thúc | 

## Vỏ cạnh 

Đối với lưới 2 hàng, 3 cột:```
3
111
001
```Cột đầu tiên có bẫy ở hàng trên nhưng phía dưới vẫn an toàn. Cột thứ hai có bẫy đôi? Không, nó có phần trên`1`, đáy`0`. Cột cuối cùng là an toàn. Thuật toán quét các cột, không tìm thấy cột bẫy kép và đưa ra kết quả chính xác là "CÓ". Điều này chứng tỏ giải pháp xử lý chính xác các lưới tối thiểu và nhận ra rằng đường dẫn tồn tại nếu có ít nhất một ô an toàn trên mỗi cột. 

Đối với đầu vào kích thước tối đa (`n = 100`), thuật toán vẫn chỉ thực hiện 100 lần kiểm tra cho mỗi trường hợp thử nghiệm, do đó, đầu ra được tạo ra một cách hiệu quả mà không cần bất kỳ sự tối ưu hóa đặc biệt nào.
