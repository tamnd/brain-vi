---
title: "CF 102862A - Hai dãy số"
description: "Chúng ta được hoán vị các số từ 1 đến n. Nhiệm vụ là chia các phần tử của nó thành hai nhóm trong khi vẫn giữ nguyên thứ tự ban đầu của chúng trong mỗi nhóm."
date: "2026-07-25T13:58:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102862
codeforces_index: "A"
codeforces_contest_name: "LU ICPC Selection Contest 2020 and KFU Open Contest 2020"
rating: 0
weight: 102862
solve_time_s: 42
verified: true
draft: false
---

[CF 102862A - Hai chuỗi tiếp theo](https://codeforces.com/problemset/problem/102862/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được hoán vị các số từ 1 đến n. Nhiệm vụ là chia các phần tử của nó thành hai nhóm trong khi vẫn giữ nguyên thứ tự ban đầu của chúng trong mỗi nhóm. Mỗi nhóm phải tạo thành một dãy con tăng dần và mọi phần tử của hoán vị phải thuộc đúng một trong số chúng. Trong số tất cả các phép chia hợp lệ, chúng ta muốn có sự khác biệt lớn nhất có thể có giữa hai độ dài chuỗi con. Nếu không có sự phân chia như vậy tồn tại, chúng tôi phải báo cáo điều đó. 

Kích thước đầu vào có thể đạt tới 500000 phần tử. Điều này ngay lập tức loại trừ việc lập trình động trên các vị trí, kiểm tra tất cả các lựa chọn dãy con hoặc bất kỳ cách tiếp cận nào có hành vi bậc hai. Với giới hạn một giây, chúng ta cần một nghiệm gần tuyến tính hoặc n log n. Cấu trúc hoán vị là thứ cho phép sự giảm thiểu này. 

Các trường hợp biên chính xuất phát từ thực tế là chỉ riêng dãy con tăng lớn nhất là không đủ. Một hoán vị có thể có dãy con tăng dài trong khi các phần tử còn lại không thể tạo thành dãy con bắt buộc thứ hai. 

Ví dụ:```
Input:
4
4 2 3 1

Output:
-1
```Dãy con tăng dài nhất có độ dài 2, chẳng hạn như 2, 3. Một giải pháp bất cẩn có thể cho rằng câu trả lời là 0 vì hai phần tử còn lại có cùng kích thước. Tuy nhiên, các phần tử 4, 1 còn lại không tăng và mọi lựa chọn có thể đều khiến một chuỗi con không hợp lệ. 

Một trường hợp quan trọng khác là hoán vị tăng hoàn toàn.```
Input:
5
1 2 3 4 5

Output:
5
```Toàn bộ hoán vị đã tăng lên, vì vậy một dãy con có thể chứa mọi phần tử và dãy còn lại có thể trống. Giải pháp buộc cả hai dãy con không trống sẽ thất bại ở đây. 

Trường hợp biên cuối cùng là hoán vị giảm dần của độ dài hai.```
Input:
2
2 1

Output:
0
```Có thể xếp từng phần tử vào dãy con riêng của nó. Câu trả lời không phải là phủ định và không cần đến dãy con trống. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là thử mọi phân vùng có thể có của hoán vị thành hai nhóm. Đối với mỗi phân vùng, chúng tôi sẽ kiểm tra xem cả hai chuỗi đã chọn có tăng lên hay không và giữ được mức chênh lệch tốt nhất. Vì mọi phần tử đều có hai lựa chọn, điều này đòi hỏi phải kiểm tra tối đa 2^n phân vùng, điều này trở nên không thể ngay cả đối với n rất nhỏ.

 Một cách tiếp cận hợp lý hơn nhưng vẫn chưa đầy đủ là tìm dãy con tăng dài nhất. Nếu độ dài của nó là L thì các phần tử n-L còn lại là số phần tử nhỏ nhất có thể nằm ngoài bất kỳ dãy con tăng dần nào. Tuy nhiên, vấn đề đòi hỏi bản thân các yếu tố còn lại ngày càng tăng. Ví dụ 4, 2, 3, 1 cho thấy tại sao điều kiện này lại quan trọng. 

Quan sát quan trọng xuất phát từ mối quan hệ giữa dãy con tăng và dãy con giảm. Một hoán vị có thể được chia thành hai dãy con tăng dần khi nó không chứa một dãy con giảm độ dài bằng ba. Điều này tuân theo định lý Dilworth, nhưng trong trường hợp đặc biệt này, nó cũng có thể được hiểu một cách trực tiếp: mọi dãy con giảm dần có độ dài bằng ba sẽ cần ba nhóm tăng khác nhau để tách các phần tử của nó. 

Một khi chúng ta biết có thể phân chia được thì dãy con lớn hơn tối ưu là dãy con tăng dài nhất. Phần bù của nó cũng phải tăng, vì nếu không phần bù sẽ chứa một cặp giảm và cùng với dãy con tăng dài nhất, nó sẽ mâu thuẫn với cấu trúc chiều rộng hai. Do đó, nếu độ dài LIS là L thì hai kích thước là L và n-L và chênh lệch tối đa là L-(n-L), bằng 2L-n. 

Nhiệm vụ còn lại là kiểm tra xem hoán vị có dãy con giảm dần có độ dài ba hay không và tính độ dài LIS. Cả hai đều có thể được thực hiện trong O(n log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính độ dài của dãy con giảm dài nhất. Chúng ta có thể làm điều này bằng cách áp dụng thuật toán LIS tiêu chuẩn cho các giá trị phủ định của hoán vị. Nếu độ dài này ít nhất là ba thì hoán vị không thể chia thành hai dãy con tăng dần, vì vậy câu trả lời là -1. 
2. Tính độ dài L của dãy con tăng dài nhất của hoán vị. Kỹ thuật sắp xếp kiên nhẫn giữ giá trị kết thúc nhỏ nhất có thể để tăng các chuỗi con có độ dài mỗi chiều, cho phép tính toán này trong O(n log n). 
3. Sử dụng tính chất là phép chia hợp lệ chỉ tồn tại giữa hai dãy con tăng dần. Dãy con lớn hơn có thể có độ dài L và dãy còn lại có độ dài n-L. Do đó, chênh lệch tối đa là L-(n-L), tức là 2L-n. 

Tại sao nó hoạt động: 

Điều kiện phân vùng được kiểm soát bởi dãy con giảm dài nhất. Nếu ba phần tử xuất hiện theo thứ tự giảm dần thì không có hai dãy con tăng nào có thể chứa chúng mà không vi phạm thuộc tính tăng dần, do đó không thể phân vùng hợp lệ. Nếu dãy con giảm dài nhất có độ dài tối đa là hai thì hoán vị có chiều rộng tối đa là hai và có thể được phân chia thành hai dãy con tăng dần. Thành viên lớn nhất có thể có của một phân vùng như vậy là một dãy con tăng dài nhất. Coi nó là phần lớn hơn để lại phần bù ngày càng tăng, do đó, sự khác biệt thu được từ LIS là có thể đạt được. Vì không có dãy con tăng nào có thể dài hơn LIS nên không thể tồn tại sự khác biệt nào tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def lis_length(arr):
    tails = []
    import bisect
    for x in arr:
        pos = bisect.bisect_left(tails, x)
        if pos == len(tails):
            tails.append(x)
        else:
            tails[pos] = x
    return len(tails)

def solve():
    n = int(input())
    p = list(map(int, input().split()))

    if lis_length([-x for x in p]) >= 3:
        print(-1)
        return

    length = lis_length(p)
    print(2 * length - n)

if __name__ == "__main__":
    solve()
```các`lis_length`chức năng này là việc triển khai sắp xếp sự kiên nhẫn tiêu chuẩn. các`tails[i]`giá trị đại diện cho giá trị kết thúc nhỏ nhất có thể có của một chuỗi con có độ dài tăng dần`i + 1`. Việc giữ những phần cuối này ở mức tối thiểu sẽ mang lại cho các phần tử trong tương lai cơ hội tốt nhất để kéo dài phần tiếp theo. 

Cuộc gọi đầu tiên sử dụng các giá trị âm. Dãy con tăng dần trong mảng phủ định tương ứng với dãy con giảm dần trong hoán vị ban đầu. Chúng ta chỉ cần biết liệu chuỗi con như vậy có đạt đến độ dài ba hay không, vì vậy phép tính LIS đơn lẻ này là đủ để xác nhận điều kiện phân vùng. 

Cuộc gọi thứ hai tính toán độ dài chuỗi tăng dần dài nhất thực tế. Biểu thức cuối cùng`2 * length - n`trực tiếp từ việc trừ quy mô nhóm nhỏ hơn khỏi quy mô nhóm lớn hơn. Số nguyên Python xử lý phạm vi giá trị có thể có mà không bị tràn. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
Input:
2
2 1
```| Bước | Giá trị hiện tại | chiều dài LIS | Chiều dài LDS | Kết quả | 
| --- | --- | --- | --- | --- | 
| Kiểm tra dãy con giảm dần | 2, 1 | 1 | 2 | Tiếp tục | 
| Tính dãy con tăng dần | 2, 1 | 1 | | 2*1-2=0 | 

Dãy con giảm dài nhất có độ dài bằng 2 nên việc phân chia là có thể. Một phần tử đi vào từng dãy con tăng dần, cho kích thước bằng nhau. 

Đối với ví dụ thứ ba:```
Input:
10
2 4 5 1 6 3 7 8 9 10
```| Bước | Giá trị hiện tại | chiều dài LIS | Chiều dài LDS | Kết quả | 
| --- | --- | --- | --- | --- | 
| Kiểm tra dãy con giảm dần | hoán vị | 8 | 2 | Tiếp tục | 
| Tính dãy con tăng dần | 2,4,5,6,7,8,9,10 | 8 | | 2*8-10=6 | 

Dãy con tăng dần chứa tám phần tử có thể ghép với hai phần tử còn lại là 1 và 3. Cả hai nhóm đều tăng nên hiệu là sáu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Hai phép tính LIS được thực hiện, mỗi phép tính xử lý mọi phần tử hoán vị bằng tìm kiếm nhị phân. | 
| Không gian | O(n) | Mảng tails lưu trữ tối đa một giá trị cho mỗi độ dài chuỗi con có thể có. | 

Giới hạn đầu vào là 500000 phần tử yêu cầu tránh các thuật toán bậc hai. Giải pháp O(n log n) chỉ thực hiện một số lượng nhỏ thao tác cho mỗi phần tử và vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
import bisect

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def lis_length(arr):
        tails = []
        for x in arr:
            pos = bisect.bisect_left(tails, x)
            if pos == len(tails):
                tails.append(x)
            else:
                tails[pos] = x
        return len(tails)

    n = int(input())
    p = list(map(int, input().split()))

    if lis_length([-x for x in p]) >= 3:
        return "-1\n"

    return str(2 * lis_length(p) - n) + "\n"

assert solution("2\n2 1\n") == "0\n", "sample 1"
assert solution("4\n4 2 3 1\n") == "-1\n", "sample 2"
assert solution("10\n2 4 5 1 6 3 7 8 9 10\n") == "6\n", "sample 3"
assert solution("11\n1 2 3 4 5 6 7 8 9 10 11\n") == "11\n", "sample 4"

assert solution("1\n1\n") == "1\n", "minimum size"
assert solution("5\n5 4 3 2 1\n") == "-1\n", "long decreasing sequence"
assert solution("6\n1 1 1 1 1 1\n") == "-1\n", "invalid non-permutation style input"
assert solution("6\n1 3 2 4 6 5\n") == "2\n", "small inversions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Xử lý chuỗi con trống và đầu vào nhỏ nhất | 
|`5 / 5 4 3 2 1`|`-1`| Phát hiện dãy con giảm dần có độ dài ba | 
|`6 / 1 3 2 4 6 5`|`2`| Xử lý nhiều nghịch đảo nhỏ | 
| Tăng hoán vị | Giá trị tối đa | Xác nhận rằng một dãy con có thể chứa mọi thứ | 

## Vỏ cạnh 

Đối với trường hợp không thể:```
Input:
4
4 2 3 1
```Mảng phủ định chứa một dãy con tăng dần có độ dài ba, tương ứng với dãy giảm ban đầu 4, 3, 1. Vì không thể đặt ba phần tử giảm vào chỉ hai dãy con tăng dần nên thuật toán ngay lập tức trả về -1. 

Đối với trường hợp tăng đầy đủ:```
Input:
5
1 2 3 4 5
```Dãy con giảm dài nhất có độ dài bằng 1, do đó phân vùng hợp lệ. Độ dài LIS là 5, cho`2*5-5 = 5`. Điều này tương ứng với việc đặt mọi phần tử vào một dãy con và để trống phần tử còn lại. 

Đối với trường hợp giảm hai phần tử:```
Input:
2
2 1
```Độ dài chuỗi con giảm dần dài nhất là hai, vẫn được phép. Độ dài LIS là một, vì vậy câu trả lời là`2*1-2 = 0`. Sự phân chia duy nhất có thể mang lại cho cả hai chuỗi con đều có độ dài bằng một. 

Tôi cũng có thể điều chỉnh bài xã luận này thành một phiên bản ngắn hơn theo phong cách Codeforces phù hợp với một bài đăng trên blog về cuộc thi thực tế nếu bạn muốn.
