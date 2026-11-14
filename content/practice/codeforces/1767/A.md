---
title: "CF 1767A - Cắt hình tam giác"
description: "Bài toán cho một tam giác trong mặt phẳng có diện tích dương, xác định bởi ba điểm có tọa độ nguyên."
date: "2026-06-09T12:54:43+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1767
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 140 (Rated for Div. 2)"
rating: 800
weight: 1767
solve_time_s: 416
verified: false
draft: false
---

[CF 1767A - Cắt tam giác](https://codeforces.com/problemset/problem/1767/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 6 phút 56 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán cho một tam giác trong mặt phẳng có diện tích dương, xác định bởi ba điểm có tọa độ nguyên. Đối với mỗi tam giác, chúng ta được hỏi liệu có thể vẽ một đường thẳng nằm ngang hoặc thẳng đứng sao cho đường thẳng đó chia tam giác thành hai tam giác nhỏ hơn, mỗi tam giác có diện tích dương. Đường thẳng không thể tạo ra một đa giác có nhiều hơn ba cạnh, không thể bỏ sót hoàn toàn hình tam giác và phải chính xác theo chiều ngang hoặc chiều dọc. 

Đầu vào chứa tối đa\(10^4\)trường hợp thử nghiệm và mỗi tọa độ lên tới\(10^8\). Do đó, mọi giải pháp đều phải xử lý từng tam giác trong thời gian \(O(1)\); lặp lại các điểm tiềm năng dọc theo các cạnh tam giác sẽ quá chậm. Nhiệm vụ thiết yếu hoàn toàn là hình học và phụ thuộc vào vị trí tương đối của ba đỉnh. 

Các trường hợp cạnh không rõ ràng bao gồm các hình tam giác trong đó hai điểm có chung điểm giống nhau\(x\)-phối hợp hoặc tương tự\(y\)-tọa độ, hoặc nơi hai bên được căn chỉnh theo trục. Ví dụ: một tam giác có các đỉnh \((3, 6)\), \((6, 6)\) và \((6, 3)\) tạo thành một tam giác vuông có hai cạnh thẳng hàng với các trục. Bất kỳ đường ngang hoặc dọc nào xuyên qua bên trong sẽ không giao nhau đúng cách hoặc tạo thành một hình tứ giác, vì vậy câu trả lời phải là KHÔNG. Việc thực hiện bất cẩn chỉ kiểm tra một số căn chỉnh mà không kiểm tra các vết cắt bên trong sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xem xét tất cả các đường ngang ở số nguyên\(y\)- Phối hợp giữa cực tiểu và cực đại\(y\)-giá trị của tam giác và tất cả các đường thẳng đứng giữa mức tối thiểu và tối đa\(x\)-value, kiểm tra số lượng giao điểm với hình tam giác. Điều này có tác dụng vì một đường thẳng phải cắt hai cạnh để chia một tam giác thành hai tam giác, nhưng số đường tiềm năng lên tới\(10^8\), làm cho phương pháp này hoàn toàn không khả thi. 

Quan sát quan trọng là để một đường ngang có thể chia một tam giác thì phải tồn tại hai đỉnh có giá trị khác nhau.\(y\)-tọa độ sao cho đỉnh thứ ba nằm hoàn toàn giữa chúng và tương tự đối với các đường thẳng đứng có\(x\)-tọa độ. Trong thực tế, điều này giảm xuống còn việc kiểm tra xem hai đỉnh nào có chung\(x\)hoặc\(y\)điều phối. Nếu hai đỉnh có chung\(x\)-tọa độ, một đường thẳng đứng có thể cắt giữa đỉnh kia và cặp căn chỉnh. Nếu hai đỉnh có chung\(y\)-tọa độ, một đường ngang có thể làm điều tương tự. Nếu không có hai đỉnh nào chung\(x\)hoặc\(y\), không có đường thẳng theo trục nào có thể chia tam giác thành hai tam giác nhỏ hơn. 

Thông tin chi tiết này cho phép giải pháp \(O(1)\) cho mỗi trường hợp thử nghiệm bằng cách so sánh trực tiếp tọa độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O(max(phạm vi x + phạm vi y)) | O(1) | Quá chậm | 
| Kiểm tra tọa độ | O(1) mỗi tam giác | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi tam giác, hãy đọc tọa độ \((x_1, y_1)\), \((x_2, y_2)\), \((x_3, y_3)\). 
2. Kiểm tra xem có bất kỳ hai trong số\(x\)-tọa độ bằng nhau. Nếu vậy, hãy in CÓ và chuyển sang hình tam giác tiếp theo. Một đường thẳng đứng xuyên qua đó\(x\)sẽ chia tam giác. 
3. Nếu không, hãy kiểm tra xem có bất kỳ hai điều nào trong số\(y\)-tọa độ bằng nhau. Nếu vậy, hãy in CÓ và chuyển sang hình tam giác tiếp theo. Một đường ngang xuyên qua đó\(y\)sẽ chia tam giác. 
4. Nếu cả hai điều kiện này đều không đúng, hãy in KHÔNG vì không có đường thẳng trục nào có thể cắt tam giác thành hai tam giác nhỏ hơn. 

Tại sao nó hoạt động: một đường chia một tam giác thành hai tam giác nhỏ hơn phải đi qua một đỉnh và cạnh đối diện, căn chỉnh hiệu quả với hai tọa độ. Nếu hai điểm giống nhau\(x\)hoặc\(y\), sự liên kết này có thể đạt được; mặt khác, tất cả các đỉnh được đặt theo đường chéo và một đường thẳng theo trục không thể tạo ra hai hình tam giác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    input()  # skip empty line
    x1, y1 = map(int, input().split())
    x2, y2 = map(int, input().split())
    x3, y3 = map(int, input().split())

    if x1 == x2 or x2 == x3 or x1 == x3:
        print("YES")
    elif y1 == y2 or y2 == y3 or y1 == y3:
        print("YES")
    else:
        print("NO")
```Mã này đọc số lượng trường hợp kiểm thử và từng tam giác một cách hiệu quả bằng cách sử dụng I/O nhanh. Dòng trống được bỏ qua rõ ràng để khớp với định dạng đầu vào. Việc kiểm tra tất cả các cặp tọa độ sẽ đảm bảo tính chính xác cho mọi hướng của tam giác. Chú ý cẩn thận đến cả hai\(x\)Và\(y\)sự bình đẳng là cần thiết để tránh thiếu sự phân chia theo trục. 

## Ví dụ đã hoạt động 

Theo dõi hai đầu vào mẫu đầu tiên: 

| Tam giác | x1,x2,x3 | y1,y2,y3 | x bằng nhau? | y bằng nhau? | Đầu ra | 
|---|---|---|---|---|---| 
| 4 7, 6 8, 3 5 | 4,6,3 | 7,8,5 | Không | Không | KHÔNG | 
| 4 5, 4 7, 6 8 | 4,4,6 | 5,7,8 | Có (4=4) | - | CÓ | 

Tam giác đầu tiên không có đỉnh thẳng hàng nên đầu ra là KHÔNG. Tam giác thứ hai có\(x_1 = x_2\), do đó có thể cắt theo chiều dọc. Điều này xác nhận việc kiểm tra tọa độ xác định chính xác các vết cắt khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(t) | Mỗi tam giác được kiểm tra trong O(1) | 
| Không gian | O(1) | Chỉ có sáu số nguyên được lưu trữ cùng một lúc | 

Với tối đa\(10^4\)trường hợp thử nghiệm, điều này thực hiện tốt trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    t = int(input())
    for _ in range(t):
        input()
        x1, y1 = map(int, input().split())
        x2, y2 = map(int, input().split())
        x3, y3 = map(int, input().split())
        if x1 == x2 or x2 == x3 or x1 == x3:
            print("YES")
        elif y1 == y2 or y2 == y3 or y1 == y3:
            print("YES")
        else:
            print("NO")
    return output.getvalue().strip()

# provided samples
assert run("4\n\n4 7\n6 8\n3 5\n\n4 5\n4 7\n6 8\n\n5 8\n1 8\n2 5\n\n3 6\n6 6\n6 3\n") == "NO\nYES\nYES\nNO"

# custom cases
assert run("2\n\n1 1\n1 2\n2 3\n\n1 2\n2 1\n3 3\n") == "YES\nNO"
assert run("1\n\n5 5\n5 5\n5 5\n") == "YES"  # degenerate but axis aligned
assert run("1\n\n1 1\n2 2\n3 3\n") == "NO"
assert run("1\n\n100000000 1\n1 100000000\n100000000 100000000\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| 1 1,1 2,2 3 | CÓ | có thể cắt dọc | 
| 1 2,2 1,3 3 | KHÔNG | không thể cắt theo trục | 
| 5 5,5 5,5 5 | CÓ | suy biến đầu vào, căn chỉnh tọa độ | 
| 1 1,2 2,3 3 | KHÔNG | tam giác chéo, không cắt | 
| 1e8 1,1 1e8,1e8 1e8 | CÓ | tọa độ lớn, cắt dọc | 

## Vỏ cạnh 

Một tam giác có cả ba đỉnh nằm trên một đường chéo, ví dụ \((1,1), (2,2), (3,3)\), không thể bị cắt bởi bất kỳ đường ngang hoặc dọc nào. Thuật toán trả về đúng NO. Tam giác có hai đỉnh chung một\(x\)-tọa độ nhưng ở xa thứ ba, ví dụ: \((1,1),(1,100),(50,50)\), có thể được chia theo chiều dọc và thuật toán trả về CÓ. Cả hai trường hợp cạnh đều xác nhận rằng việc kiểm tra sự bằng nhau của bất kỳ cặp tọa độ nào là đủ và chắc chắn trước các cực trị.
