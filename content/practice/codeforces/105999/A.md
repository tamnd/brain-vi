---
title: "CF 105999A - Ace Race"
description: "Bài toán mô phỏng một cuộc đua trên một trục số vô hạn. Alice bắt đầu ở vị trí a. Giải thưởng xuất hiện ở một trong hai vị trí có thể là x hoặc y và Bob phải chọn một vị trí số nguyên khác bắt đầu trước khi biết vị trí nào xảy ra."
date: "2026-06-25T13:20:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105999
codeforces_index: "A"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge \u57f9\u6b63\u5587\u6c99\u7de8\u7a0b\u6311\u6230\u8cfd 2024"
rating: 0
weight: 105999
solve_time_s: 39
verified: true
draft: false
---

[CF 105999A - Cuộc đua Át](https://codeforces.com/problemset/problem/105999/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô phỏng một cuộc đua trên một trục số vô hạn. Alice bắt đầu ở vị trí`a`. Giải thưởng xuất hiện ở một trong hai vị trí có thể,`x`hoặc`y`, và Bob phải chọn một vị trí bắt đầu số nguyên khác trước khi biết số nào xảy ra. Bob thắng nếu vị trí đã chọn của anh ấy gần với cả hai vị trí có thể nhận được giải thưởng hơn vị trí ban đầu của Alice. 

Đối với mỗi trường hợp thử nghiệm, đầu vào cung cấp ba vị trí`a`,`x`, Và`y`. Đầu ra hỏi liệu có tồn tại vị trí xuất phát hợp lệ cho Bob để đảm bảo chiến thắng hay không. Các vị trí nhỏ nhưng số lượng ca kiểm thử có thể lớn, vì vậy giải pháp nên tránh khám phá quá nhiều vị trí có thể có cho mỗi ca kiểm thử. Với tối đa hàng nghìn trường hợp, phương pháp kiểm tra số lượng ứng viên không đổi là lý tưởng, trong khi việc quét trên phạm vi tọa độ lớn sẽ không cần thiết. 

Khó khăn chính là Bob cần một vị trí có thể đánh bại Alice ở cả hai điểm đến cùng một lúc. Việc chỉ kiểm tra một điểm đến có thể gây hiểu nhầm vì một điểm gần với`x`có thể quá xa`y`. 

Một sai lầm phổ biến là quên rằng Bob không thể xuất phát ở vị trí của Alice. Ví dụ:```
1
5 3 1
```Đầu ra đúng là:```
YES
```Một cách tiếp cận bất cẩn có thể cố gắng luôn chọn điểm giữa hoặc chọn cạnh đối diện của Alice mà không kiểm tra khoảng cách chính xác. Ở đây Bob có thể chọn vị trí`2`, đó là khoảng cách`1`từ cả hai mục tiêu. Alice là khoảng cách`2`từ`3`và khoảng cách`4`từ`1`. 

Một trường hợp khó khăn khác là khi Alice đã ở giữa hai vị trí giải thưởng có thể có:```
1
3 1 5
```Đầu ra đúng là:```
NO
```Bất kỳ vị trí nào di chuyển gần hơn`1`di chuyển xa hơn từ`5`, và bất kỳ vị trí nào di chuyển gần hơn đến`5`di chuyển xa hơn từ`1`. Vì Alice ở chính giữa nên Bob không thể đánh bại cô ấy ở cả hai bên. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là thử mọi vị trí bắt đầu số nguyên có thể có cho Bob. Đối với từng vị trí ứng viên`b`, chúng tôi kiểm tra xem:```
|b - x| < |a - x|
and
|b - y| < |a - y|
```Nếu một số vị trí thỏa mãn cả hai bất đẳng thức thì câu trả lời là có. Điều này đúng vì các điều kiện mô tả trực tiếp yêu cầu chiến thắng. 

Vấn đề là quyết định có bao nhiêu vị trí để kiểm tra. Nếu tọa độ lớn thì việc kiểm tra mọi điểm có thể sẽ quá chậm. Mặc dù phiên bản cụ thể này có tọa độ nhỏ, nhưng quan sát dự kiến ​​là câu trả lời chỉ phụ thuộc vào vị trí`a`nằm tương đối với`x`Và`y`. 

Hai vị trí mục tiêu chia đường thành các vùng. Nếu Alice không ở giữa hai mục tiêu, Bob có thể chọn một điểm gần giữa khoảng cách giữa hai mục tiêu hơn.`x`Và`y`, vì hướng đó làm giảm cả hai khoảng cách. Nếu Alice ở giữa hai mục tiêu, cô ấy đã ở bên tốt nhất có thể cho một trong các mục tiêu và bất kỳ chuyển động nào cũng khiến một bên trở nên tồi tệ hơn. 

Cái nhìn sâu sắc quan trọng là điều duy nhất quan trọng là liệu`a`nằm ngoài đoạn được tạo bởi`x`Và`y`. Nếu như`a`ở bên ngoài, việc di chuyển một bước về phía đoạn đó sẽ giảm khoảng cách đến cả hai điểm cuối. Nếu như`a`ở bên trong, không có chuyển động như vậy tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(phạm vi tọa độ) | O(1) | Không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba vị trí`a`,`x`, Và`y`. 
2. Tìm vị trí giải thưởng nhỏ hơn và lớn hơn. Hãy để họ như vậy`low`Và`high`. Điều này đưa ra phân đoạn mà giải thưởng có thể xuất hiện. 
3. Kiểm tra xem vị trí của Alice có nằm ngoài đoạn này hay không. Nếu như`a < low`hoặc`a > high`, Bob có thể tiến một bước tới phân khúc đó và tiến gần hơn đến cả hai giải thưởng có thể có. 
4. In`YES`nếu Alice ở ngoài phân đoạn, nếu không thì in`NO`. 

Lý do điều này hoạt động dựa trên cách hoạt động của khoảng cách trên một đường. Khi Alice ở bên ngoài hai mục tiêu, mọi chuyển động về phía khoảng cách sẽ làm giảm cả hai khoảng cách. Khi cô ấy ở bên trong, hai mục tiêu ở hai phía đối diện nhau, vì vậy việc cải thiện khoảng cách này đòi hỏi phải làm xấu đi khoảng cách kia. 

Tại sao nó hoạt động: 

Điều bất biến là Bob cần một vị trí gần với mọi vị trí có thể có giải thưởng hơn. Đoạn giữa`x`Và`y`chứa tất cả các vị trí cân bằng giữa hai mục tiêu. Từ bên ngoài đoạn này di chuyển vào trong sẽ làm giảm khoảng cách đến cả hai đầu. Từ bên trong đoạn đường, mọi vị trí đều đã nằm giữa các mục tiêu nên không có hướng nào có thể giảm cả hai khoảng cách cùng một lúc. Điều này khớp chính xác với điều kiện được kiểm tra bởi thuật toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        a, x, y = map(int, input().split())

        low = min(x, y)
        high = max(x, y)

        if a < low or a > high:
            ans.append("YES")
        else:
            ans.append("NO")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc số lượng trường hợp kiểm thử và xử lý từng trường hợp một cách độc lập. Đối với mọi trường hợp, nó tạo ra khoảng chứa hai vị trí giải thưởng có thể có. 

điều kiện`a < low or a > high`kiểm tra xem Alice có nằm ngoài khoảng đó không. Nếu đúng như vậy, Bob có thể tiến vào trong và cải thiện cả hai khoảng cách. Nếu không, Alice nằm giữa các mục tiêu và Bob không thể đảm bảo chiến thắng. 

Không có vòng lặp trên các vị trí, do đó không có mối lo ngại về ranh giới hoặc tràn. Chi tiết triển khai duy nhất cần tránh là so sánh với`x`Và`y`trực tiếp mà không cần sắp xếp chúng, vì hai vị trí mục tiêu có thể xuất hiện theo một trong hai thứ tự. 

## Ví dụ đã hoạt động 

Mẫu 1:```
3
1 3 4
5 3 1
3 1 5
```Đối với trường hợp đầu tiên: 

| một | thấp | cao | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 3 | 4 | Bên ngoài, CÓ | 

Alice nằm ngoài cả hai mục tiêu. Di chuyển sang phải giúp Bob đến gần hơn với cả hai giải thưởng có thể có. 

Đối với trường hợp thứ hai: 

| một | thấp | cao | Quyết định | 
| --- | --- | --- | --- | 
| 5 | 1 | 3 | Bên ngoài, CÓ | 

Alice đúng về cả hai mục tiêu. Di chuyển sang trái cải thiện cả hai khoảng cách. 

Đối với trường hợp thứ ba: 

| một | thấp | cao | Quyết định | 
| --- | --- | --- | --- | 
| 3 | 1 | 5 | Bên trong, KHÔNG | 

Alice nằm giữa hai vị trí giải thưởng có thể có nên không có vị trí Bob nào chiếm ưu thế. 

Mẫu 2: 

đầu vào:```
1
10 2 7
```Dấu vết: 

| một | thấp | cao | Quyết định | 
| --- | --- | --- | --- | 
| 10 | 2 | 7 | Bên ngoài, CÓ | 

Bob có thể di chuyển sang trái từ`10`về phía khoảng. Khoảng cách đến cả hai`2`Và`7`trở nên nhỏ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm chỉ thực hiện một vài so sánh. | 
| Không gian | O(1) | Chỉ các giá trị trường hợp thử nghiệm hiện tại được lưu trữ, ngoài danh sách đầu ra. | 

Lời giải dễ dàng phù hợp với các giới hạn vì nó không phụ thuộc vào kích thước của phạm vi tọa độ. Ngay cả một số lượng rất lớn các trường hợp thử nghiệm cũng chỉ yêu cầu một lượng công việc không đổi cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        a, x, y = map(int, input().split())
        low = min(x, y)
        high = max(x, y)

        if a < low or a > high:
            ans.append("YES")
        else:
            ans.append("NO")

    return "\n".join(ans)

# provided samples
assert solve("""3
1 3 4
5 3 1
3 1 5
""") == """YES
YES
NO""", "sample cases"

# minimum style case
assert solve("""1
1 2 3
""") == "YES", "outside left boundary"

# inside interval
assert solve("""1
5 1 10
""") == "NO", "cannot beat middle"

# reversed target order
assert solve("""1
10 8 2
""") == "YES", "target ordering"

# multiple cases with same answer
assert solve("""3
4 1 2
2 1 3
8 3 7
""") == """YES
NO
YES""", "mixed cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 2 3`|`YES`| Alice ở bên ngoài ranh giới bên trái. | 
|`1 / 5 1 10`|`NO`| Alice đang ở trong khoảng thời gian có thể nhận được giải thưởng. | 
|`1 / 10 8 2`|`YES`| Vị trí mục tiêu có thể xuất hiện theo bất kỳ thứ tự nào. | 
|`3 / mixed cases`|`YES NO YES`| Nhiều trường hợp thử nghiệm độc lập. | 

## Vỏ cạnh 

Đối với trường hợp:```
1
5 3 1
```Thuật toán sắp xếp các vị trí giải thưởng thành`low = 1`Và`high = 3`. Alice đang ở`5`, lớn hơn`high`, vậy cô ấy ở ngoài phân khúc. Thuật toán trả về`YES`. Bob có thể chọn vị trí`2`, cho khoảng cách`1`Và`1`, trong khi Alice có khoảng cách`2`Và`4`. 

Đối với trường hợp:```
1
3 1 5
```Khoảng được sắp xếp là`[1, 5]`. Alice chính xác là ở bên trong nó. Thuật toán trả về`NO`. Bất kỳ chuyển động nào về phía mục tiêu bên trái sẽ tăng khoảng cách đến mục tiêu bên phải và chuyển động về phía mục tiêu bên phải sẽ tăng khoảng cách đến mục tiêu bên trái. 

Đối với các trường hợp khoảng cách bằng nhau như:```
1
0 2 4
```Alice cũng ở giữa các mục tiêu nên câu trả lời là`NO`. Điểm quan trọng là việc đứng chính xác ở giữa không tạo được lợi thế cho Bob. Một điểm khởi đầu duy nhất không thể cải thiện cả hai mặt cùng một lúc.
