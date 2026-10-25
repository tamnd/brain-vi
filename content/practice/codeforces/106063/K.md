---
title: "CF 106063K - Giao lộ km"
description: "Bài toán yêu cầu chúng ta đo xem hai đoạn đường trên một trục số trùng nhau bao nhiêu. Mỗi đoạn là một khoảng khép kín, do đó nó bao gồm cả hai điểm cuối, nhưng câu trả lời dựa trên độ dài, nghĩa là một điểm tiếp xúc duy nhất không đóng góp gì."
date: "2026-06-25T12:15:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106063
codeforces_index: "K"
codeforces_contest_name: "2025 ICPC Gran Premio de Mexico 3ra Fecha"
rating: 0
weight: 106063
solve_time_s: 34
verified: true
draft: false
---

[CF 106063K - Giao lộ Kilometric](https://codeforces.com/problemset/problem/106063/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta đo xem hai đoạn đường trên một trục số trùng nhau bao nhiêu. Mỗi đoạn là một khoảng khép kín, do đó nó bao gồm cả hai điểm cuối, nhưng câu trả lời dựa trên độ dài, nghĩa là một điểm tiếp xúc duy nhất không đóng góp gì. Cho hai khoảng`[a, b]`Và`[c, d]`, chúng ta cần xuất ra độ dài của phần chung của chúng hoặc bằng 0 khi phần chung không có độ dài dương. 

Đầu vào chứa nhiều trường hợp thử nghiệm độc lập. Số trường hợp có thể lên tới`10^5`và tọa độ có thể lớn bằng`10^18`về giá trị tuyệt đối. Điều này ngay lập tức loại trừ bất cứ điều gì khác ngoài công việc hằng số hoặc logarit cho mỗi trường hợp, bởi vì`10^5`các trường hợp chỉ còn chỗ cho khoảng vài triệu thao tác đơn giản. Kích thước tọa độ cũng có nghĩa là việc triển khai phải sử dụng số học số nguyên có thể xử lý các giá trị 64 bit. 

Các trường hợp cạnh chính xuất phát từ việc giao điểm khó hiểu là một tập hợp các điểm có giao điểm là chiều dài. Ví dụ:```
1
0 1 1 2
```Các khoảng gặp nhau ở tọa độ`1`, nhưng phần chung chỉ là điểm`{1}`. Độ dài của nó bằng 0 nên đáp án là:```
0
```Một giải pháp chỉ kiểm tra xem các khoảng chồng chéo có thể trả về không chính xác hay không`1`. 

Một trường hợp khác là khi một khoảng hoàn toàn nằm trong một khoảng khác:```
1
0 10 3 7
```Giao lộ là`[3,7]`, có độ dài là`4`. Việc triển khai bất cẩn chỉ kiểm tra một điểm cuối có thể bỏ sót tình huống này. 

Trường hợp khó khăn cuối cùng là khi cả hai khoảng đều là điểm:```
1
5 5 5 5
```Các khoảng giống nhau nhưng độ dài của chúng vẫn bằng 0 vì không có khoảng cách giữa các điểm cuối. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là so sánh các khoảng thời gian và tìm mọi tình huống chồng chéo có thể xảy ra. Chúng ta có thể kiểm tra xem một khoảng có bắt đầu bên trong khoảng khác hay không, liệu khoảng này có chứa khoảng kia hay không, hay chúng có rời rạc hay không. Điều này có tác dụng vì mọi mối quan hệ có thể có giữa hai khoảng đều thuộc một trong các loại này. Tuy nhiên, điều này tạo ra sự phân nhánh không cần thiết và dễ mắc sai lầm khi chạm vào điểm cuối. 

Cấu trúc của các khoảng cho chúng ta một quan sát đơn giản hơn. Ranh giới bên trái của giao điểm phải là điểm sau của hai điểm cuối bên trái, vì cả hai khoảng đều phải đạt đến tọa độ đó. Tương tự, ranh giới bên phải phải là điểm đầu tiên của hai điểm cuối bên phải. Nếu ranh giới bên trái thu được nhỏ hơn ranh giới bên phải thì có một giao điểm có độ dài dương. Ngược lại, giao điểm có độ dài bằng 0. 

Kiểu phân tích trường hợp mạnh mẽ cũng hoạt động trong thời gian không đổi, nhưng việc quan sát tối thiểu và tối đa làm giảm logic thành một công thức duy nhất. Vấn đề không phải là tìm kiếm hay mô phỏng mà chỉ là xác định hai ranh giới của vùng chia sẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Chấp nhận được nhưng dễ mắc lỗi hơn | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bốn điểm cuối của hai khoảng. 

Chúng ta chỉ cần các điểm cuối vì giao điểm của hai khoảng hoàn toàn được xác định bởi ranh giới của chúng. 
2. Tính phía bên trái của giao lộ là`max(a, c)`. 

Bất kỳ điểm nào trong giao điểm đều phải thuộc cả hai khoảng, vì vậy nó không thể thuộc về một trong hai khoảng. 
3. Tính phía bên phải của giao lộ là`min(b, d)`. 

Bất kỳ điểm nào trong giao lộ phải kết thúc trước khi một trong hai đoạn kết thúc. 
4. Nếu bên trái nhỏ hơn bên phải, hãy in ra hiệu số giữa chúng. Nếu không thì xuất ra số 0. 

Cần phải có sự bất đẳng thức nghiêm ngặt vì các ranh giới bằng nhau có nghĩa là giao điểm chỉ là một điểm duy nhất. 

Tại sao nó hoạt động: thuật toán duy trì phạm vi chính xác có thể trong đó cả hai khoảng có thể chứa một điểm cùng một lúc. Điểm bắt đầu tối đa sẽ loại bỏ mọi vị trí không có sẵn trong một trong các khoảng và điểm kết thúc tối thiểu sẽ loại bỏ mọi vị trí nằm ngoài một trong các khoảng. Đoạn còn lại chính xác là giao lộ. Nếu nó không có chiều rộng dương thì chiều dài của nó bằng 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        a, b, c, d = map(int, input().split())

        left = max(a, c)
        right = min(b, d)

        if left < right:
            ans.append(str(right - left))
        else:
            ans.append("0")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các trường hợp kiểm thử và xử lý từng trường hợp một cách độc lập. Các biến`left`Và`right`đại diện cho hai giá trị duy nhất cần thiết để mô tả giao điểm. 

Việc so sánh sử dụng`<`thay vì`<=`. Điều này xử lý chính xác trường hợp chạm vào điểm cuối, bởi vì giao lộ như`[2,2]`có độ dài bằng không. Số nguyên Python có độ chính xác tùy ý, do đó tọa độ lớn không yêu cầu bất kỳ xử lý đặc biệt nào. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3 7 1 5
```việc thực thi trông như thế này: 

| Bước | trái | đúng | hành động | 
| --- | --- | --- | --- | 
| Khoảng thời gian ban đầu | - | - |`[3,7]`Và`[1,5]`| 
| Tìm điểm bắt đầu giao lộ | 3 | - | tối đa(3,1) | 
| Tìm điểm cuối giao lộ | 3 | 5 | phút(7,5) | 
| Kiểm tra lần cuối | 3 | 5 | đầu ra 5-3 = 2 | 

Giao lộ là`[3,5]`, vậy câu trả lời là`2`. Điều này thể hiện trường hợp chồng chéo thông thường. 

Đối với đầu vào:```
0 1 1 2
```việc thực hiện là: 

| Bước | trái | đúng | hành động | 
| --- | --- | --- | --- | 
| Khoảng thời gian ban đầu | - | - |`[0,1]`Và`[1,2]`| 
| Tìm điểm bắt đầu giao lộ | 1 | - | tối đa(0,1) | 
| Tìm điểm cuối giao lộ | 1 | 1 | phút(1,2) | 
| Kiểm tra lần cuối | 1 | 1 | đầu ra 0 | 

Hai khoảng chạm nhau nhưng không chia sẻ bất kỳ khoảng cách nào. Điều này xác nhận tại sao sự bình đẳng phải tạo ra số không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm sử dụng một số lượng so sánh cố định | 
| Không gian | O(T) | Chuỗi đầu ra được lưu trữ hoặc O(1) bổ sung bên cạnh đầu ra | 

Với tối đa`10^5`trong các trường hợp thử nghiệm, việc quét tuyến tính dễ dàng nằm trong giới hạn vì mỗi trường hợp được giảm xuống còn một vài phép tính số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        a, b, c, d = map(int, input().split())
        left = max(a, c)
        right = min(b, d)
        ans.append(str(right - left if left < right else 0))

    return "\n".join(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

assert run("""6
3 7 1 5
0 0 0 0
-5 -1 2 8
1 10 3 6
-1 4 4 10
-10 -2 -7 0
""") == """2
0
0
3
0
0
5""", "sample"

assert run("""1
5 5 5 5
""") == "0", "point intervals"

assert run("""1
0 10 3 7
""") == "4", "contained interval"

assert run("""1
-1000000000000000000 1000000000000000000 -999999999999999999 0
""") == "999999999999999999", "large coordinates"

assert run("""1
0 5 6 10
""") == "0", "disjoint intervals"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[5,5]`Và`[5,5]`|`0`| Khoảng suy biến | 
|`[0,10]`Và`[3,7]`|`4`| Một khoảng chứa đầy khoảng khác | 
| Điểm cuối rất lớn | Độ dài hợp lệ lớn | Xử lý ranh giới 64-bit | 
|`[0,5]`Và`[6,10]`|`0`| Không chồng chéo | 

## Vỏ cạnh 

Đối với trường hợp điểm cuối chạm:```
1
0 1 1 2
```thuật toán tính toán`left = max(0,1) = 1`Và`right = min(1,2) = 1`. Vì hai giá trị bằng nhau nên điều kiện không thành công và câu trả lời trở thành 0. Thuật toán xử lý chính xác điểm được chia sẻ là không có độ dài. 

Đối với một khoảng chứa:```
1
0 10 3 7
```giao lộ được tính toán là từ`3`ĐẾN`7`, bởi vì bắt đầu muộn hơn là`3`và kết thúc trước đó là`7`. Câu trả lời là`7 - 3 = 4`, cho thấy rằng không cần logic ngăn chặn đặc biệt. 

Đối với tọa độ âm:```
1
-10 -2 -7 0
```giao lộ là`[-7,-2]`. Thuật toán tìm`left = -7`Và`right = -2`, cho`5`. Các vị trí phủ định hoạt động giống hệt như các vị trí tích cực vì chỉ có thứ tự mới quan trọng.
