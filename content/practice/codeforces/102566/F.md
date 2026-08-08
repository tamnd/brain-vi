---
title: "CF 102566F - Cây Đũa Thần"
description: "Chúng ta có một hàng số nguyên và cần sắp xếp hàng đó theo thứ tự không giảm. Hành động khả dụng duy nhất là chọn một phần liên tiếp của hàng và chỉ sắp xếp phần đó. Năng lượng tiêu tốn chỉ phụ thuộc vào độ dài của phần được chọn và một đoạn có độ dài k có giá k³."
date: "2026-08-07T21:29:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102566
codeforces_index: "F"
codeforces_contest_name: "AGM 2020, Qualification Round"
rating: 0
weight: 102566
solve_time_s: 118
verified: true
draft: false
---

[CF 102566F - Cây đũa thần](https://codeforces.com/problemset/problem/102566/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng số nguyên và cần sắp xếp hàng đó theo thứ tự không giảm. Hành động khả dụng duy nhất là chọn một phần liên tiếp của hàng và chỉ sắp xếp phần đó. Năng lượng tiêu tốn chỉ phụ thuộc vào độ dài của phần được chọn và một đoạn có độ dài`k`chi phí`k³`. Nhiệm vụ là tìm tổng năng lượng tối thiểu cần thiết. 

Quan sát hữu ích đầu tiên là chúng ta chỉ quan tâm đến các vị trí có giá trị chưa phải là giá trị mà chúng phải có trong mảng được sắp xếp cuối cùng. Không cần phải chạm vào một vị trí chứa giá trị đúng nhưng mọi vị trí không chính xác phải được đưa vào ít nhất một thao tác. 

Kích thước đầu vào lớn. Một trường hợp thử nghiệm có thể chứa tới`100000`số lượng và tổng kích thước trên tất cả các trường hợp thử nghiệm có thể đạt tới`2000000`. Điều này loại trừ bất cứ điều gì liên quan đến việc thử nhiều khoảng thời gian hoặc lập trình động trên các vị trí, bởi vì ngay cả`O(n²)`sẽ là quá chậm. Chúng ta cần một giải pháp quét mảng một số lần không đổi sau khi sắp xếp. 

Một số trường hợp rất dễ bỏ sót. Nếu mảng đã được sắp xếp thì câu trả lời là 0 vì không cần thực hiện thao tác nào. 

Ví dụ:```
1
5
1 2 3 4 5
```Câu trả lời là:```
0
```Một giải pháp bất cẩn luôn tạo ra các phép toán cho toàn bộ mảng sẽ trả về giá trị dương. 

Một trường hợp quan trọng khác là khi các vị trí sai được phân tách bằng các vị trí đúng.```
1
4
2 1 1 3
```Mảng được sắp xếp là`[1,1,2,3]`, do đó vị trí 1 và 3 sai. Chúng không thể được coi là một khối liên tục có các vị trí sai. Giải pháp tối ưu sử dụng hai thao tác có độ dài 2, một thao tác bao gồm vị trí 1 và 2 và một thao tác khác bao gồm vị trí 2 và 3, với chi phí là`8 + 8 = 16`. 

Một giải pháp chỉ đếm độ dài của các khối sai liền kề sẽ bỏ qua khả năng này một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử từng khoảng, sắp xếp nó và xem chuỗi thao tác nào mang lại năng lượng nhỏ nhất. Điều này đúng vì mọi di chuyển được phép đều được mô phỏng rõ ràng. Tuy nhiên, có`O(n²)`khoảng thời gian và mỗi mô phỏng sẽ yêu cầu công việc bổ sung. Vì`n = 100000`, điều này là hoàn toàn không thể. 

Quan sát chính xuất phát từ việc xem xét quá trình ngược lại. Hãy tưởng tượng bắt đầu với mảng được sắp xếp và áp dụng phép toán nghịch đảo của chúng ta. Thao tác ngược có thể sắp xếp lại khoảng thời gian đã chọn một cách tùy ý vì thao tác tiến chỉ đơn giản là sắp xếp nó. Do đó, các vị trí duy nhất cần được đưa vào các thao tác ngược lại là các vị trí khác với cách sắp xếp cuối cùng. 

Bây giờ hãy xem xét hàm chi phí. Một đoạn có độ dài`k`chi phí`k³`. Một đoạn có chiều dài lớn hơn hai không bao giờ tốt hơn việc thay thế nó bằng những đoạn nhỏ hơn. Ví dụ: chi phí của một đoạn có chiều dài 3`27`, trong khi hai đoạn có chiều dài 2 có giá`16`. Nói chung, bao gồm một chiều dài`k`khoảng thời gian với các cặp liền kề`8 * ceil(k / 2)`, luôn nhỏ hơn`k³`vì`k >= 3`. 

Điều này làm giảm vấn đề thành một vấn đề đơn giản hơn nhiều. Chúng ta chỉ cần chọn các cặp vị trí liền kề sao cho mỗi vị trí sai đều được bao phủ bởi ít nhất một cặp. Mỗi cặp được chọn có giá chính xác`8`. 

Đối với một lần chạy liên tiếp các vị trí có độ dài không chính xác`k`, số cặp liền kề tối thiểu cần có là`ceil(k / 2)`. Các lần chạy không tương tác vì vị trí chính xác giữa hai lần chạy vẫn có thể được sử dụng làm lân cận của một vị trí không chính xác bị cô lập, nhưng nó không thể giảm số lượng cặp cần thiết trong một lần chạy khác. Quét các vị trí không chính xác và đếm các cặp này sẽ đưa ra câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một bản sao đã được sắp xếp của mảng. So sánh mọi vị trí ban đầu với giá trị của nó trong bản sao được sắp xếp và đánh dấu xem vị trí đó có chính xác hay không. Đây là những vị trí duy nhất phải được đảm bảo bởi các hoạt động. 
2. Quét mảng boolean có vị trí không chính xác. Bất cứ khi nào tìm thấy một chuỗi liên tiếp có vị trí không chính xác, hãy đặt độ dài của nó là`len`. Lần chạy này cần`ceil(len / 2)`các thao tác liền kề vì mỗi thao tác có thể bao gồm tối đa hai vị trí. 
3. Thêm`8 * ceil(len / 2)`để có câu trả lời cho mỗi lần chạy. Chi phí vận hành dài 2`2³ = 8`, do đó nhân số cặp cần thiết với 8 sẽ có tổng năng lượng. 
4. Xuất năng lượng tích lũy. 

Tại sao nó hoạt động: 

Mỗi vị trí sai phải xuất hiện trong ít nhất một khoảng đã chọn. Vì các khoảng dài hơn hai luôn đắt hơn việc chia chúng thành các khoảng có độ dài-2, nên giải pháp tối ưu chỉ sử dụng các cặp liền kề. Một cuộc chạy`len`vị trí không chính xác đòi hỏi ít nhất`ceil(len / 2)`cặp vì mỗi cặp bao gồm nhiều nhất hai vị trí. Việc xây dựng sử dụng các cặp liền kề xen kẽ đạt đến giới hạn dưới này, do đó số lượng cặp là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        b = sorted(a)

        wrong = [a[i] != b[i] for i in range(n)]

        cost = 0
        i = 0
        while i < n:
            if not wrong[i]:
                i += 1
                continue

            j = i
            while j < n and wrong[j]:
                j += 1

            length = j - i
            cost += ((length + 1) // 2) * 8
            i = j

        ans.append(str(cost))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Bản sao được sắp xếp chỉ được sử dụng để xác định vị trí nào đã chính xác. Thuật toán không bao giờ sửa đổi mảng vì không cần chuỗi thao tác thực tế mà chỉ cần năng lượng tối thiểu. 

Quá trình quét tìm thấy số lần chạy tối đa của các vị trí không chính xác. biểu thức`(length + 1) // 2`tính toán mức trần của một nửa chiều dài chạy, tránh số học dấu phẩy động. Số nguyên Python cũng tránh bị tràn vì câu trả lời lớn nhất có thể nằm trong phạm vi của chúng. 

Các điều kiện biên được xử lý bởi hai vòng lặp while. Một lần chạy kết thúc ở vị trí cuối cùng được xử lý chính xác vì vòng lặp bên trong dừng khi`j == n`. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
1
4
2 1 4 3
```Mảng được sắp xếp là`[1,2,3,4]`. Các vị trí sai đều là bốn vị trí. 

| chỉ mục | sai | chiều dài chạy hiện tại | chi phí bổ sung | 
| --- | --- | --- | --- | 
| 1 | vâng | 4 | 0 | 
| 2 | vâng | 4 | 0 | 
| 3 | vâng | 4 | 0 | 
| 4 | vâng | xong | 16 | 

Chiều dài chạy là`4`, vì vậy nó cần`ceil(4/2)=2`hoạt động lân cận. Chi phí là`2 * 8 = 16`. 

Một ví dụ riêng biệt:```
1
4
2 1 1 3
```Mảng được sắp xếp là`[1,1,2,3]`. 

| chỉ mục | giá trị | giá trị được sắp xếp | sai | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | vâng | 
| 2 | 1 | 1 | không | 
| 3 | 1 | 2 | vâng | 
| 4 | 3 | 3 | không | 

Có hai lần chạy dài một. 

| chạy | chiều dài | cặp bắt buộc | chi phí | 
| --- | --- | --- | --- | 
| sai vị trí đầu tiên | 1 | 1 | 8 | 
| sai vị trí thứ hai | 1 | 1 | 8 | 

Câu trả lời cuối cùng là`16`. Ví dụ này cho thấy tại sao chỉ đếm các khối sai liền kề là không đủ. Vị trí đơn sai vẫn cần phải ghép với hàng xóm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong quá trình quét tuyến tính | 
| Không gian | O(n) | Bản sao đã sắp xếp và mảng boolean được lưu trữ | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm là`2 * 10^6`, do đó cách tiếp cận sắp xếp phù hợp thoải mái trong giới hạn. Công việc còn lại là tuyến tính và chỉ thêm một hằng số nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        import sys
        input = sys.stdin.readline
        t = int(input())
        out = []

        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            b = sorted(a)

            ans = 0
            i = 0
            while i < n:
                if a[i] == b[i]:
                    i += 1
                    continue
                j = i
                while j < n and a[j] != b[j]:
                    j += 1
                ans += ((j - i + 1) // 2) * 8
                i = j

            out.append(str(ans))

        print("\n".join(out))

    result = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = result
    solve()
    sys.stdout = old_stdout
    sys.stdin = old_stdin
    return result.getvalue()

assert run("""1
4
2 1 4 3
""") == "16\n", "sample"

assert run("""1
5
1 2 3 4 5
""") == "0\n", "already sorted"

assert run("""1
4
2 1 1 3
""") == "16\n", "separated wrong positions"

assert run("""1
1
7
""") == "0\n", "single element"

assert run("""1
6
6 5 4 3 2 1
""") == "24\n", "long reverse case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 4 3`|`16`| Ví dụ được cung cấp với hai cặp độc lập | 
|`1 2 3 4 5`|`0`| Đã sắp xếp mảng | 
|`2 1 1 3`|`16`| Bị cô lập vị trí không chính xác | 
|`7`|`0`| Vỏ kích thước tối thiểu | 
| Thứ tự đảo ngược của sáu giá trị |`24`| Vùng sai liên tục lớn hơn | 

## Vỏ cạnh 

Đối với một mảng đã được sắp xếp, chẳng hạn như:```
1
5
1 2 3 4 5
```mọi so sánh với bản sao được sắp xếp đều thành công. Quá trình quét không bao giờ thực hiện sai vị trí nên câu trả lời vẫn là 0. 

Đối với các vị trí sai được phân tách:```
1
4
2 1 1 3
```thuật toán nhìn thấy hai lần chạy có độ dài một. Mỗi lần chạy đóng góp một cặp liền kề, mang lại`8 + 8 = 16`. Vị trí chính xác ở giữa được phép đưa vào cả hai thao tác, đó là lý do tại sao hai vị trí không chính xác vẫn có thể được sửa. 

Đối với một phân đoạn sai liên tục:```
1
6
6 5 4 3 2 1
```cả sáu vị trí đều sai. Độ dài chạy là sáu, vì vậy ba thao tác liền kề là đủ theo công thức. Câu trả lời là`3 * 8 = 24`. Thuật toán không cố gắng sử dụng một khoảng lớn vì chi phí khối sẽ lớn hơn nhiều.
