---
title: "CF 102785H - Chuỗi tự mô tả"
description: "Chúng ta cần xây dựng một chuỗi có độ dài k sao cho mỗi vị trí mô tả tần suất chỉ mục của nó xuất hiện trong toàn bộ chuỗi. Nếu giá trị tại vị trí i là x thì số i phải xuất hiện chính xác x lần trong số tất cả các phần tử."
date: "2026-07-27T19:42:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102785
codeforces_index: "H"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 18)"
rating: 0
weight: 102785
solve_time_s: 72
verified: true
draft: false
---

[CF 102785H - Trình tự tự mô tả](https://codeforces.com/problemset/problem/102785/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Chúng ta cần xây dựng một chuỗi có độ dài`k`sao cho mỗi vị trí mô tả tần suất chỉ mục của nó xuất hiện trong toàn bộ chuỗi. Nếu giá trị ở vị trí`i`là`x`, thì số`i`phải xảy ra chính xác`x`lần giữa tất cả các phần tử. 

Đầu vào cung cấp độ dài chuỗi, sau đó là danh sách các chỉ mục có giá trị cần thiết. Chúng ta không phải in toàn bộ dãy mà chỉ in các phần tử được yêu cầu theo đúng thứ tự. Nếu không tồn tại chuỗi hợp lệ có độ dài yêu cầu, chúng tôi sẽ in`0`. 

Chiều dài tối đa là`230`, trong khi số lượng vị trí được yêu cầu có thể đạt tới`100000`. Điều này ngay lập tức loại trừ bất cứ điều gì xây dựng nhiều chuỗi ứng cử viên hoặc quét liên tục toàn bộ chuỗi cho mọi truy vấn. Chúng ta cần tìm một chuỗi hợp lệ trong thời gian gần như tuyến tính, sau đó trả lời các truy vấn bằng cách lập chỉ mục trực tiếp. 

Phần khó khăn không phải là kích thước đầu ra mà là điều kiện tự tham chiếu. Một chuỗi có thể có vẻ hợp lệ cục bộ trong khi không thành công trên toàn cầu vì việc thay đổi một giá trị sẽ thay đổi một số tần số. 

Coi như`k = 3`. Việc thực hiện bất cẩn có thể thử`[1, 2, 0]`bởi vì nó giống với ví dụ có độ dài hợp lệ bốn. Tuy nhiên, số lượng là: số 0 xuất hiện một lần, một xuất hiện một lần, hai xuất hiện một lần. Các giá trị cần thiết sẽ là`[1, 1, 1]`, vì vậy ứng cử viên không hợp lệ. 

Một sai lầm phổ biến khác là cho rằng mỗi độ dài đều có giải pháp. Vì`k = 1`, các chuỗi duy nhất có thể là`[0]`Và`[1]`. TRONG`[0]`, số 0 xuất hiện một lần nhưng giá trị tại chỉ số 0 bằng 0. TRONG`[1]`, số 0 xuất hiện 0 lần nhưng giá trị tại chỉ số 0 là một. Đầu ra đúng là`0`. 

Chiều dài nhỏ là nơi cần xử lý đặc biệt. Cấu trúc chung chỉ bắt đầu hoạt động khi có đủ chỗ để đặt các tần số khác 0 cần thiết ở các chỉ số riêng biệt. 

# Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử các giá trị có thể có cho tất cả`k`vị trí và xác minh xem chuỗi kết quả có mô tả chính nó hay không. Vì mỗi phần tử có thể nằm giữa`0`Và`k - 1`, không gian tìm kiếm này rất lớn. Ngay cả khi cắt tỉa, trường hợp xấu nhất vẫn tăng theo cấp số nhân và không thể`k = 230`. 

Quan sát hữu ích đến từ việc xem xét tổng của tất cả các giá trị. Trình tự chứa`k`các số và những số đó là số lần xuất hiện nên tổng của chúng phải chính xác`k`. 

Cho phép`z`là số số 0 trong dãy. Bởi vì giá trị tại chỉ mục`0`cho chúng ta biết có bao nhiêu số 0 tồn tại, chúng ta có`a[0] = z`. Các mục khác 0 còn lại phải có tổng bằng`k - z`, và số lượng của chúng cũng là`k - z`. Điều này có nghĩa là tất cả các mục khác 0 còn lại đều có giá trị trung bình chính xác`1`. Cách duy nhất để có các số nguyên dương với mức trung bình đó trong khi không làm cho mọi giá trị bằng một là có một giá trị bằng hai và tất cả các giá trị khác bằng một. 

Để đủ lớn`k`, chúng ta có thể tạo ra chính xác bốn vị trí khác 0. Đặt các giá trị khác 0 đó là:`k - 4`,`2`,`1`,`1`. 

giá trị`k - 4`xuất hiện một lần, giá trị`2`xuất hiện một lần và giá trị`1`xuất hiện hai lần. Phần còn lại`k - 4`các vị trí bằng không. Do đó trình tự là:`a[0] = k - 4`

`a[1] = 2`

`a[2] = 1`

`a[k - 4] = 1`Tất cả các vị trí khác đều bằng không. 

Điều này hiệu quả với mọi`k >= 7`, bởi vì`k - 4`ít nhất là`3`, vì vậy cả bốn chỉ số đặc biệt đều khác biệt. 

Các trường hợp hợp lệ còn lại đủ nhỏ để xử lý với một danh sách cố định các câu trả lời đã biết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(k) | Quá chậm | 
| Tối ưu | O(k + n) | O(k) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Nếu`k`là`4`, sử dụng trình tự hợp lệ`[1, 2, 1, 0]`. Đây là một trong những trường hợp nhỏ không thể áp dụng cách xây dựng tổng quát do các chỉ số đặc biệt chồng chéo lên nhau. 
2. Nếu`k`ít nhất là`7`, tạo một mảng có độ dài`k`chứa đầy số không. Bộ`a[0]`ĐẾN`k - 4`, bộ`a[1]`ĐẾN`2`, bộ`a[2]`ĐẾN`1`, và đặt`a[k - 4]`ĐẾN`1`. Bốn giá trị này tạo ra chính xác tần số theo yêu cầu của định nghĩa. 
3. Đối với tất cả các độ dài khác, hãy báo cáo rằng không tồn tại trình tự tự mô tả. 
4. Nếu một chuỗi được tạo, hãy trả lời mọi chỉ mục được yêu cầu bằng cách đọc trực tiếp giá trị được lưu trữ. 

Tại sao nó hoạt động: 

Việc xây dựng có bốn giá trị khác không. Giá trị tại chỉ số 0 là`k - 4`, vậy phải có chính xác`k - 4`số không. Vì ba vị trí khác 0 còn lại chứa`2`,`1`, Và`1`, có chính xác`k - 4`không có mục nào và có đúng hai lần xuất hiện`1`, một lần xuất hiện của`2`và một lần xuất hiện của`k - 4`. Các giá trị được lưu trữ tại các chỉ số`1`,`2`, Và`k - 4`chính xác là những tần số này, vì vậy mỗi chỉ mục mô tả số lần xuất hiện của chính nó. 

#Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    k = int(input())
    n = int(input())
    queries = list(map(int, input().split()))

    ans = None

    if k == 4:
        ans = [1, 2, 1, 0]
    elif k >= 7:
        ans = [0] * k
        ans[0] = k - 4
        ans[1] = 2
        ans[2] = 1
        ans[k - 4] = 1

    if ans is None:
        print(0)
        return

    print(" ".join(str(ans[x]) for x in queries))

if __name__ == "__main__":
    solve()
```Đầu tiên chương trình quyết định liệu một chuỗi hợp lệ có tồn tại hay không. Mảng chỉ được tạo sau khi biết danh mục độ dài, điều này tránh được những công việc không cần thiết trong các trường hợp không thể thực hiện được. 

Vì`k >= 7`, thứ tự gán quan trọng vì`k - 4`phải được sử dụng làm chỉ mục. Điều kiện đảm bảo chỉ số này khác với`0`,`1`, Và`2`, vì vậy không có bài tập nào ghi đè lên nhau. 

Bước đầu ra cuối cùng chỉ thực hiện tra cứu mảng. Điều này là cần thiết vì số lượng truy vấn lớn hơn nhiều so với`k`và việc tính toán lại tần suất cho mọi yêu cầu sẽ rất lãng phí. 

# Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên, chiều dài là`4`và các chỉ số được yêu cầu đều là các vị trí. 

| Bước | k | Trình tự xây dựng | Giá trị đầu ra | 
| --- | --- | --- | --- | 
| 1 | 4 | Sử dụng trường hợp đặc biệt`[1,2,1,0]`| | 
| 2 | 4 |`[1,2,1,0]`|`1 2 1 0`| 

Dãy số này chứa một số không, hai số một và một số hai. Các số đếm đó khớp với các giá trị được lưu trữ tại các chỉ số 0, một và hai. 

Đối với ví dụ thứ hai, độ dài của`7`sử dụng kết cấu chung. 

| Bước | k | k-4 | Trình tự xây dựng | Giá trị đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 3 | Bắt đầu với tất cả các số 0 | | 
| 2 | 7 | 3 | Bộ`a[0]=3`,`a[1]=2`,`a[2]=1`,`a[3]=1`| | 
| 3 | 7 | 3 |`[3,2,1,1,0,0,0]`| Các giá trị được yêu cầu được đọc | 

Các giá trị được tính như sau: số 0 xuất hiện ba lần, một xuất hiện hai lần, hai xuất hiện một lần và ba xuất hiện một lần. Điều này phù hợp với mảng được xây dựng. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k + n) | Việc xây dựng chuỗi mất O(k) và trả lời các truy vấn mất O(n). | 
| Không gian | O(k) | Chỉ có trình tự được xây dựng được lưu trữ. | 

Chiều dài tối đa chỉ`230`, vì vậy bản thân công trình này rất nhỏ. Hoạt động chủ yếu là xử lý lên đến`100000`các chỉ số được yêu cầu, dễ dàng nằm trong giới hạn. 

# Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    k = int(input())
    n = int(input())
    queries = list(map(int, input().split()))

    ans = None
    if k == 4:
        ans = [1, 2, 1, 0]
    elif k >= 7:
        ans = [0] * k
        ans[0] = k - 4
        ans[1] = 2
        ans[2] = 1
        ans[k - 4] = 1

    if ans is None:
        out = "0"
    else:
        out = " ".join(str(ans[i]) for i in queries)

    sys.stdin = old_stdin
    return out

assert solution("4\n4\n0 1 2 3\n") == "1 2 1 0", "sample 1"
assert solution("7\n4\n0 1 2 3\n") == "3 2 1 1", "sample 2"

assert solution("1\n1\n0\n") == "0", "minimum impossible length"
assert solution("3\n3\n0 1 2\n") == "0", "small impossible length"
assert solution("4\n2\n3 0\n") == "0 1", "small valid construction"
assert solution("230\n3\n0 1 226\n") == "226 2 1", "maximum length construction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4`với tất cả các chỉ số |`1 2 1 0`| Trường hợp hợp lệ nhỏ đặc biệt | 
|`7`với chỉ số xây dựng |`3 2 1 1`| Độ dài đầu tiên áp dụng công thức | 
|`1`|`0`| Ranh giới tối thiểu và trường hợp không thể | 
|`3`|`0`| Độ dài không hợp lệ nhỏ | 
|`4`chỉ truy vấn vị trí`3`Và`0`|`0 1`| Xử lý đúng thứ tự truy vấn tùy ý | 
|`230`|`226 2 1`| Độ dài tối đa cho phép | 

# Vỏ cạnh 

cho`k = 1`, thuật toán đạt đến nhánh không thể. Không có chuỗi hợp lệ vì một phần tử không thể đồng thời bằng số lượng giá trị của chính nó và đáp ứng yêu cầu số lượng bằng 0. 

Vì`k = 4`, công thức chung sẽ cố gắng đặt giá trị đặc biệt tại chỉ mục`0`, bởi vì`k - 4 = 0`. Điều đó sẽ ghi đè lên logic xây dựng. Trình tự rõ ràng`[1,2,1,0]`tránh sự chồng chéo này và thỏa mãn định nghĩa một cách chính xác. 

Vì`k = 7`, công thức cho`k - 4 = 3`, sản xuất`[3,2,1,1,0,0,0]`. Có chính xác ba số 0, hai số một, một hai và một số ba, khớp với các giá trị tại chỉ số`0`,`1`,`2`, Và`3`. Điều này khẳng định ranh giới đầu tiên nơi công trình chung trở nên an toàn.
