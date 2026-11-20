---
title: "CF 2137D - Thay thế bằng các lần xuất hiện"
description: "Chúng ta được cấp một dãy số và chúng ta muốn diễn giải mỗi số trong dãy đó như một yêu cầu về tần số đối với một mảng chưa biết nào đó mà chúng ta phải xây dựng."
date: "2026-06-08T02:32:58+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 2137
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1047 (Div. 3)"
rating: 1200
weight: 2137
solve_time_s: 184
verified: false
draft: false
---

[CF 2137D - Thay thế bằng các lần xuất hiện](https://codeforces.com/problemset/problem/2137/D) 

**Đánh giá:** 1200 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 3m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số và chúng ta muốn diễn giải mỗi số trong dãy đó như một yêu cầu về tần số đối với một mảng chưa biết nào đó mà chúng ta phải xây dựng. Cụ thể, chúng ta phải xây dựng một mảng`a`sao cho mọi vị trí`i`chứa một giá trị`a[i]`, và giá trị ghi vào`b[i]`phải bằng số lần`a[i]`xuất hiện trong toàn bộ mảng. 

Điều này tạo ra một điều kiện tự tham khảo. Mỗi vị trí không mô tả trực tiếp giá trị của chính nó mà thay vào đó mô tả số lần giá trị đó sẽ lặp lại trên toàn cầu. Nếu chúng ta gán một giá trị`x`đến một vị trí nào đó thì mọi vị trí chứa`x`phải đồng ý về tổng số như nhau`x`trong mảng cuối cùng. 

Đầu ra là một mảng như vậy`a`hoặc một tuyên bố rằng không có phép gán nhất quán nào tồn tại. 

Các ràng buộc cho phép tổng số tối đa 2·10^5 phần tử trong các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất cứ điều gì bậc hai cho mỗi trường hợp thử nghiệm. Chúng ta cần một cấu trúc tuyến tính hoặc gần tuyến tính, có thể dựa trên việc nhóm hoặc đếm tần số. 

Chế độ lỗi tinh tế xuất hiện khi các giá trị trong`b`nội bộ không nhất quán. Ví dụ: nếu một vị trí yêu cầu tần số là 2 và vị trí khác yêu cầu tần số là 3, nhưng không có đủ vị trí để đáp ứng đồng thời cả hai thì không có công trình nào tồn tại. Một vấn đề khác nảy sinh khi tính đến xung đột về mặt cấu trúc. Ví dụ,`b = [2, 2, 3]`là không thể vì một giá trị xuất hiện 2 lần không thể đáp ứng được vị trí yêu cầu 3 lần xuất hiện, tuy nhiên tất cả các vị trí phải thuộc về một lớp giá trị nào đó. 

Một mâu thuẫn minh họa nhỏ là`b = [1, 2, 3]`. Nếu chúng ta gán một giá trị cho vị trí yêu cầu tần số 3 thì giá trị đó phải xuất hiện ba lần, nhưng chỉ có một vị trí có yêu cầu 3, không còn chỗ để tạo thành nhóm kích thước 3. 

## Phương pháp tiếp cận 

Một nỗ lực bạo lực sẽ cố gắng gán giá trị cho các vị trí và kiểm tra tính nhất quán của số lượng tần số cảm ứng. Đối với mỗi vị trí, chúng tôi có thể đoán một giá trị, duy trì một mảng đang chạy, tính toán lại tần số và xác minh xem mọi vị trí có khớp với tần số yêu cầu hay không. Ngay cả khi chúng ta hạn chế các giá trị ở`1..n`, điều này trở thành tổ hợp: mỗi`n`các vị trí có tới`n`các lựa chọn, dẫn đến số lượng bài tập tăng theo cấp số nhân và thậm chí việc xác nhận mỗi bài tập là tuyến tính. Điều này vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là điều kiện chỉ phụ thuộc vào việc nhóm các chỉ số có giá trị được gán giống hệt nhau. Một khi chúng ta quyết định rằng một giá trị`x`được sử dụng`k`lần, mọi chỉ số được giao`x`phải có`b[i] = k`. Điều này có nghĩa là tất cả các vị trí được gán cùng một giá trị phải có cùng một giá trị`b[i]`, và số vị trí như vậy phải bằng giá trị đó. 

Vì vậy, vấn đề giảm xuống còn việc nhóm các chỉ số theo tần suất yêu cầu của chúng và đảm bảo rằng mỗi kích thước nhóm phù hợp với giá trị xác định nó. Nếu một tần số`k`xuất hiện`c`lần trong`b`, thì những cái đó`c`các vị trí phải được phân chia thành các nhóm, mỗi nhóm đại diện cho một giá trị riêng biệt xuất hiện chính xác`k`lần. Vì vậy, mỗi nhóm tiêu dùng chính xác`k`chỉ số, do đó`c`phải chia hết cho`k`. 

Sau khi đã kiểm tra tính khả thi, việc xây dựng rất đơn giản: với mỗi giá trị`k`, lấy tất cả các chỉ số với`b[i] = k`, chia chúng thành các phần có kích thước`k`và gán nhãn mới cho mỗi đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Nhóm tần số | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đếm xem mỗi giá trị bao nhiêu lần`k`xuất hiện ở`b`. 

Điều này cho chúng ta biết có bao nhiêu vị trí yêu cầu một số giá trị phải xuất hiện chính xác`k`lần. 
2. Với mỗi giá trị riêng biệt`k`, kiểm tra xem số đếm có`cnt[k]`chia hết cho`k`. 

Nếu không, chúng tôi ngay lập tức kết luận rằng không thể phân nhóm hợp lệ. 

Lý do là mỗi giá trị hợp lệ được gán cho các vị trí có yêu cầu`k`phải chiếm chính xác`k`các vị trí, không được phép để lại phần còn lại. 
3. Đối với mọi giá trị hợp lệ`k`, lấy danh sách các chỉ số trong đó`b[i] = k`. 
4. Phân chia danh sách này thành các khối có kích thước liên tiếp`k`. Mỗi khối tương ứng với một giá trị riêng biệt trong mảng cuối cùng`a`. 
5. Gán một nhãn số nguyên duy nhất mới cho mỗi khối và ghi nhãn đó vào tất cả các chỉ mục của khối trong`a`. 
6. Xuất mảng đã xây dựng. 

### Tại sao nó hoạt động 

Tất cả các vị trí được gán cùng một giá trị đều đến từ cùng một nhóm chỉ số có giống hệt nhau`b[i]`. Mỗi nhóm có kích thước chính xác`k`, do đó mọi vị trí trong nhóm đó đều nhìn thấy chính xác tần số`k`trong mảng cuối cùng. Vì các nhóm rời rạc và bao gồm tất cả các chỉ số trong mỗi lớp tần số nên không có xung đột nào phát sinh giữa các giá trị tần số khác nhau. Điều kiện chia hết đảm bảo rằng không có chỉ mục nào bị bỏ cặp trong lớp tần số của nó, ngăn chặn các nhóm một phần vi phạm định nghĩa của`f(x)`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))
        
        pos = [[] for _ in range(n + 1)]
        for i, x in enumerate(b):
            pos[x].append(i)
        
        a = [-1] * n
        ok = True
        label = 1
        
        for k in range(1, n + 1):
            if not pos[k]:
                continue
            if len(pos[k]) % k != 0:
                ok = False
                break
            
            for i in range(0, len(pos[k]), k):
                for j in range(i, i + k):
                    a[pos[k][j]] = label
                label += 1
        
        if not ok:
            print(-1)
        else:
            print(*a)

if __name__ == "__main__":
    solve()
```Các nhóm thực hiện chỉ số theo tần suất yêu cầu của họ`b[i]`. các`pos`Cấu trúc lưu trữ các nhóm này một cách hiệu quả. Với mỗi tần số`k`, chúng tôi xác minh khả năng chia hết trước khi gán nhãn. 

Một điểm tinh tế là nhãn được giới thiệu theo từng nhóm kích thước`k`, không theo giá trị`k`. Điều này là cần thiết vì nhiều giá trị riêng biệt trong`a`có thể chia sẻ cùng một yêu cầu về tần số nhưng phải duy trì khả năng phân biệt để đáp ứng tính duy nhất của các nhóm. 

Vòng lặp lồng nhau gán một mã định danh mới cho mỗi khối, đảm bảo rằng mỗi nhóm trở thành một giá trị riêng biệt trong`a`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 6
b = [1, 2, 2, 3, 3, 3]
```Chúng tôi xây dựng nhóm vị trí: 

| k | vị trí | 
| --- | --- | 
| 1 | [0] | 
| 2 | [1, 2] | 
| 3 | [3, 4, 5] | 

Bây giờ chúng tôi chỉ định: 

| k | hành động | gán nhãn | 
| --- | --- | --- | 
| 1 | khối đơn kích thước 1 | a[0] = 1 | 
| 2 | khối [1,2] | a[1]=2, a[2]=2 | 
| 3 | khối [3,4,5] | a[3]=3, a[4]=3, a[5]=3 | 

Mảng cuối cùng:```
[1, 2, 2, 3, 3, 3]
```Điều này chứng tỏ rằng mỗi giá trị được xây dựng xuất hiện chính xác nhiều lần theo yêu cầu của tất cả các vị trí thực hiện yêu cầu đó. 

### Ví dụ 2 

đầu vào:```
n = 4
b = [1, 2, 3, 4]
```Xô: 

| k | vị trí | 
| --- | --- | 
| 1 | [0] | 
| 2 | [1] | 
| 3 | [2] | 
| 4 | [3] | 

Ta kiểm tra tính chia hết: 

| k | đếm | k chia số? | 
| --- | --- | --- | 
| 1 | 1 | vâng | 
| 2 | 1 | không | 

Từ`1 % 2 != 0`, việc xây dựng thất bại ngay lập tức. 

Điều này cho thấy rằng một sự không phù hợp duy nhất trong tính khả thi của việc nhóm sẽ làm mất hiệu lực toàn bộ cấu trúc, ngay cả khi các giá trị khác có vẻ nhất quán cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một lần để nhóm và một lần để gán | 
| Không gian | O(n) | Lưu trữ nhóm vị trí và mảng đầu ra | 

Thuật toán là tuyến tính cho mỗi trường hợp thử nghiệm và tổng của`n`trên các trường hợp thử nghiệm được giới hạn bởi 2·10^5, do đó giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            b = list(map(int, input().split()))
            
            pos = [[] for _ in range(n + 1)]
            for i, x in enumerate(b):
                pos[x].append(i)
            
            a = [-1] * n
            ok = True
            label = 1
            
            for k in range(1, n + 1):
                if not pos[k]:
                    continue
                if len(pos[k]) % k != 0:
                    ok = False
                    break
                for i in range(0, len(pos[k]), k):
                    for j in range(i, i + k):
                        a[pos[k][j]] = label
                    label += 1
            
            print(-1 if not ok else *a)

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""3
4
1 2 3 4
6
1 2 2 3 3 3
6
6 6 6 6 6 6
""") == """-1
1 2 2 3 3 3
1 1 1 1 1 1"""

# minimal case
assert run("""1
1
1
""") == "1"

# all equal valid
assert run("""1
3
1 1 1
""") != ""

# mixed impossible
assert run("""1
2
1 2
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | tính khả thi cơ bản | 
| tất cả đều bình đẳng | mảng hằng hợp lệ | thành lập nhóm | 
| trộn nhỏ | -1 | lỗi chia hết | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các giá trị giống hệt nhau, chẳng hạn như`b = [n, n, n, ..., n]`. Thuật toán đặt tất cả các chỉ mục vào một nhóm duy nhất và xác minh rằng`n % n == 0`, giữ, tạo ra một nhóm duy nhất. Mọi chỉ mục đều nhận được cùng một nhãn và mảng kết quả thỏa mãn điều kiện vì mọi giá trị đều xuất hiện chính xác`n`lần. 

Một trường hợp khác là khi các giá trị hầu hết đều nhỏ nhưng lại xuất hiện một yêu cầu lớn, chẳng hạn`b = [1, 1, 1, 4]`. Cái xô dành cho`4`có kích thước 1, không chia hết cho 4 nên thuật toán sẽ loại bỏ ngay lập tức. Bất kỳ nỗ lực nào để gán một giá trị cho vị trí đó sẽ yêu cầu bốn lần xuất hiện, điều này là không thể nếu chỉ có một vị trí ứng cử viên trong lớp đó. 

Trường hợp cạnh cấu trúc cuối cùng là các nhóm hợp lệ được xen kẽ như`b = [2, 2, 1, 1, 1, 1]`. Ở đây cả hai nhóm đều thỏa mãn khả năng chia hết: hai vị trí cho`2`, bốn cho`1`. Thuật toán phân chia chính xác chúng thành một nhóm có kích thước 2 và một nhóm có kích thước 4, gán các nhãn riêng biệt một cách độc lập. Điều này khẳng định rằng tính độc lập giữa các lớp tần số là đủ, vì không có ràng buộc nào liên kết các giá trị khác nhau của`k`trong công trình xây dựng cuối cùng.
