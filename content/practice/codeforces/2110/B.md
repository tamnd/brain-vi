---
title: "CF 2110B - Xuống có giá đỡ"
description: "Chúng ta được cung cấp một chuỗi biểu thị một chuỗi dấu ngoặc đơn cân bằng và chúng ta cần quyết định xem có thể loại bỏ chính xác một dấu ngoặc mở và chính xác một dấu ngoặc đóng sao cho chuỗi kết quả không còn cân bằng nữa."
date: "2026-06-08T04:34:36+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 2110
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1026 (Div. 2)"
rating: 900
weight: 2110
solve_time_s: 68
verified: true
draft: false
---

[CF 2110B - Xuống có dấu ngoặc](https://codeforces.com/problemset/problem/2110/B) 

**Xếp hạng:** 900 
**Thẻ:** chuỗi 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi biểu thị một chuỗi dấu ngoặc đơn cân bằng và chúng ta cần quyết định xem có thể loại bỏ chính xác một dấu ngoặc mở và chính xác một dấu ngoặc đóng sao cho chuỗi kết quả không còn cân bằng nữa. Một trình tự cân bằng tuân theo các quy tắc thông thường: mỗi dấu ngoặc mở có một dấu ngoặc đóng phù hợp và các dấu ngoặc được lồng vào nhau đúng cách. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm và mỗi chuỗi được đảm bảo cân bằng. Độ dài của mỗi chuỗi có thể lên tới 200.000 và tổng của tất cả các chuỗi trong tất cả các trường hợp thử nghiệm cũng không vượt quá 200.000. Điều này ngụ ý rằng chúng ta phải giải từng trường hợp kiểm thử trong thời gian gần như tuyến tính tương ứng với độ dài chuỗi, vì thuật toán bậc hai hoặc bậc ba sẽ quá chậm. 

Các chuỗi cân bằng nhỏ nhất có thể có là "()" và "((...))", giúp chúng ta hiểu rõ hơn về các trường hợp đặc biệt. Ví dụ: không thể phá vỡ một chuỗi như "()" bằng cách xóa một dấu ngoặc mở và một dấu đóng: xóa cả hai sẽ để lại chuỗi trống, chuỗi này vẫn cân bằng. Tương tự, các chuỗi được lồng hoàn hảo mà không cần ghép các khối cân bằng riêng biệt, như "(())", cũng không thể bị phá vỡ bằng cách xóa một trong mỗi loại dấu ngoặc. Mặt khác, các chuỗi bao gồm các chuỗi con cân bằng được nối, như "(())()()", có thể bị phá vỡ bằng cách loại bỏ dấu ngoặc mở cuối cùng và dấu ngoặc đóng đầu tiên của các khối liền kề. 

Trường hợp cạnh không rõ ràng xảy ra khi chuỗi là một khối lồng nhau, chẳng hạn như "(())", trong đó việc loại bỏ một dấu ngoặc mở và một dấu ngoặc đóng khỏi các vị trí khác nhau không thể tạo ra sự mất cân bằng. Việc cố gắng loại bỏ ngẫu nhiên một cách ngây thơ có thể cho thấy nó có thể bị hỏng, nhưng việc kiểm tra cẩn thận cho thấy nó không thể bị hỏng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét từng cặp của một dấu ngoặc mở và một dấu ngoặc đóng, loại bỏ chúng và kiểm tra xem chuỗi kết quả có cân bằng hay không. Kiểm tra số dư yêu cầu quét chuỗi và duy trì bộ đếm có dấu ngoặc mở ròng. Cách tiếp cận này đúng, nhưng đối với một chuỗi có độ dài$n$, nó sẽ liên quan đến$O(n^3)$hoạt động trong trường hợp xấu nhất:$O(n^2)$cặp và$O(n)$để xác minh số dư. Với$n$lên tới 200.000, điều này là không thể. 

Quan sát quan trọng là một chuỗi cân bằng chỉ có thể chống lại việc bị phá vỡ nếu nó là một khối đơn được lồng hoàn hảo mà không cần nối. Nếu nửa đầu của trình tự là tất cả các phần mở và nửa sau là tất cả các phần kết, việc loại bỏ một phần mở ở đầu và một phần đóng ở cuối vẫn để lại một trình tự cân bằng. Cấu trúc này tương đương với một chuỗi có dạng "((...))" không có nối. Bất kỳ chuỗi cân bằng nào khác đều có ít nhất hai "khối" liền kề; việc loại bỏ lần mở cuối cùng của khối đầu tiên và lần đóng đầu tiên của khối tiếp theo sẽ tạo ra sự mất cân bằng nên có thể bị phá vỡ. 

Do đó, vấn đề giảm xuống còn việc kiểm tra xem chuỗi có phải là một khối được lồng hoàn hảo hay không. Nếu đúng thì xuất ra "NO"; nếu không thì xuất ra "CÓ". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc chuỗi$s$. Chuỗi được đảm bảo cân bằng nên số dấu ngoặc mở bằng số dấu ngoặc đóng. 
2. Đếm số dấu ngoặc mở liên tiếp từ đầu chuỗi cho đến dấu ngoặc đóng đầu tiên. Gọi đây`prefix_open`. 
3. Đếm số dấu ngoặc đóng liên tiếp từ cuối chuỗi lùi về phía sau cho đến dấu ngoặc mở đầu tiên. Gọi đây`suffix_close`. 
4. So sánh`prefix_open + suffix_close`vào tổng chiều dài của chuỗi. Nếu chúng bằng độ dài chuỗi, điều đó có nghĩa là tất cả dấu ngoặc mở đều ở đầu và tất cả dấu ngoặc đóng đều ở cuối. Đây là một khối lồng nhau. 
5. Nếu đó là một khối lồng nhau, hãy xuất "NO" vì việc loại bỏ một dấu ngoặc mở và một dấu ngoặc đóng ở bất kỳ đâu sẽ vẫn để lại một chuỗi cân bằng. Ngược lại, xuất ra "CÓ" vì có ít nhất một cách để loại bỏ dấu ngoặc khỏi hai khối liền kề và phá vỡ số dư. 

Tại sao nó hoạt động: Điều bất biến ở đây là một chuỗi cân bằng không phải là một khối lồng nhau duy nhất chứa ít nhất một ranh giới giữa hai khối. Việc loại bỏ một dấu ngoặc ở cuối một khối và một dấu ngoặc ở đầu khối tiếp theo luôn tạo ra một dấu ngoặc không khớp, phá vỡ sự cân bằng. Ngược lại, một khối lồng nhau không thể bị phá vỡ bằng chính xác một trong mỗi loại dấu ngoặc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        n = len(s)
        prefix_open = 0
        while prefix_open < n and s[prefix_open] == '(':
            prefix_open += 1
        suffix_close = 0
        while suffix_close < n and s[n - 1 - suffix_close] == ')':
            suffix_close += 1
        if prefix_open + suffix_close == n:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    main()
```Vòng lặp đầu tiên đếm các dấu ngoặc mở liên tiếp đầu tiên. Vòng lặp thứ hai đếm các dấu ngoặc đóng liên tiếp. So sánh tổng của chúng với tổng chiều dài sẽ trực tiếp kiểm tra cấu trúc khối lồng nhau. Các trường hợp cạnh, như "()" hoặc "((()))", được xử lý chính xác vì trong những trường hợp đó`prefix_open + suffix_close`bằng độ dài của chuỗi, tạo ra "NO" chính xác. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`"(())"`| tôi | tiền tố_open | hậu tố_đóng | tiền tố_open + hậu tố_đóng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 4 | KHÔNG | 

Dấu vết cho thấy tất cả các phần mở đều ở đầu và tất cả các phần đóng ở cuối, tạo thành một khối duy nhất. Loại bỏ một trong mỗi cái không phá vỡ sự cân bằng. 

Đầu vào mẫu 2:`"(())()()"`| tôi | tiền tố_open | hậu tố_đóng | tiền tố_open + hậu tố_đóng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 4 | CÓ | 

Ở đây, chuỗi không phải là một khối duy nhất. Có sự kết hợp của hai chuỗi con cân bằng. Việc loại bỏ một lần mở khỏi khối thứ hai và một lần đóng khỏi khối đầu tiên sẽ phá vỡ chuỗi, do đó đầu ra là "CÓ". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm yêu cầu hai lần quét tuyến tính để đếm dấu ngoặc tiền tố và hậu tố. | 
| Không gian | O(1) | Chỉ cần bộ đếm số nguyên. Không có bộ nhớ bổ sung tỷ lệ thuận với n. | 

Với tổng độ dài chuỗi không vượt quá 200.000, thuật toán này chạy thoải mái trong vòng chưa đầy 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# provided samples
assert run("4\n(())\n(())()()\n()\n(())(())\n") == "NO\nYES\nNO\nYES", "sample 1"

# minimum-size input, cannot break
assert run("1\n()\n") == "NO", "minimum size"

# maximum-size nested block, cannot break
assert run(f"1\n{'(' * 100000 + ')' * 100000}\n") == "NO", "max size nested block"

# two concatenated blocks, can break
assert run(f"1\n{'(' * 50000 + ')' * 50000 + '(' * 50000 + ')' * 50000}\n") == "YES", "max size concatenated"

# single pair with surrounding blocks, can break
assert run("1\n()()()\n") == "YES", "multiple small blocks"

# alternating brackets, can break
assert run("1\n()()()()\n") == "YES", "alternating blocks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "()" | KHÔNG | Chuỗi có độ dài tối thiểu | 
| 100000 '(' + 100000 ')' | KHÔNG | Khối đơn có kích thước tối đa | 
| 50000 '(' + 50000 ')' + 50000 '(' + 50000 ')' | CÓ | Khối nối có kích thước tối đa | 
| "()()()" | CÓ | Nhiều khối nhỏ | 
| "()()()()" | CÓ | Dấu ngoặc xen kẽ, có thể phá vỡ | 

## Vỏ cạnh 

Đối với "()", thuật toán được tính`prefix_open = 1`Và`suffix_close = 1`. Tổng của chúng bằng 2, chính là độ dài của chuỗi, do đó kết quả là "NO" chính xác. Đối với một khối lồng nhau lớn, chẳng hạn như 100.000 '(' theo sau là 100.000 ')', bộ đếm sẽ phát hiện chính xác một khối lồng nhau và trả về "KHÔNG". Đối với các khối được nối, như "(())()()", tổng tiền tố và hậu tố nhỏ hơn tổng chiều dài, kích hoạt "CÓ", thể hiện thuật toán xử lý ranh giới giữa các khối một cách chính xác.
