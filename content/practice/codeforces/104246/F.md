---
title: "CF 104246F - Tìm phần thưởng từ RAPL"
description: "Chúng tôi được cấp một tập hợp các lập trình viên, mỗi người bắt đầu với một số giá trị phần thưởng ban đầu. Theo thời gian, một chuỗi các bản cập nhật xuất hiện và mỗi bản cập nhật nhắm mục tiêu chính xác vào một lập trình viên và tăng phần thưởng của lập trình viên đó lên một số nguyên, có thể dương hoặc âm."
date: "2026-07-01T23:01:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104246
codeforces_index: "F"
codeforces_contest_name: "CodeSmash 2021 by RAPL"
rating: 0
weight: 104246
solve_time_s: 80
verified: false
draft: false
---

[CF 104246F - Tìm phần thưởng từ RAPL](https://codeforces.com/problemset/problem/104246/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một tập hợp các lập trình viên, mỗi người bắt đầu với một số giá trị phần thưởng ban đầu. Theo thời gian, một chuỗi các bản cập nhật xuất hiện và mỗi bản cập nhật nhắm mục tiêu chính xác vào một lập trình viên và tăng phần thưởng của lập trình viên đó lên một số nguyên, có thể dương hoặc âm. 

Sau mỗi lần cập nhật, chúng tôi được yêu cầu báo cáo có bao nhiêu giá trị phần thưởng riêng biệt tồn tại trong số tất cả các lập trình viên tại thời điểm đó. Nói cách khác, sau khi xử lý k bản cập nhật đầu tiên, về mặt khái niệm, chúng tôi có một mảng được cập nhật có kích thước n và chúng tôi muốn số lượng giá trị duy nhất bên trong nó. 

Cấu trúc đầu vào là nhiều trường hợp thử nghiệm độc lập. Mỗi trường hợp kiểm thử đưa ra một mảng ban đầu, sau đó là một luồng sửa đổi điểm và chúng ta phải xuất ra một câu trả lời cho mỗi bước cập nhật. 

Các ràng buộc quan trọng vì cả n và m có thể lớn tới 100000 cho mỗi trường hợp thử nghiệm và tổng số trên tất cả các trường hợp thử nghiệm nhiều nhất là 200000. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào tính toán lại số lượng giá trị riêng biệt từ đầu sau mỗi lần cập nhật, vì điều đó sẽ tốn O(n) cho mỗi truy vấn và dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất. 

Một vấn đề khó nhận thấy là các giá trị có thể lặp lại nhiều sau khi cập nhật. Một trực giác ngây thơ có thể cố gắng duy trì bản đồ tần số của các giá trị, nhưng việc cập nhật tần số một cách mù quáng mà không theo dõi xem có bao nhiêu phần tử chia sẻ một giá trị sẽ dẫn đến số lượng khác biệt không chính xác. 

Một trường hợp cạnh khác xuất hiện khi nhiều bộ mã hóa hội tụ về cùng một giá trị sau các đường dẫn cập nhật khác nhau. Ví dụ: bắt đầu bằng [1, 2], sau đó thêm +1 vào chỉ mục 1 và -1 vào chỉ mục 2 sẽ mang lại [2, 1], vẫn có hai giá trị riêng biệt ngay cả khi các giá trị riêng lẻ đã thay đổi danh tính. Bất kỳ giải pháp nào cũng phải theo dõi tính đa dạng của giá trị toàn cầu, không chỉ lịch sử trên mỗi chỉ mục. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp sẽ tính toán lại số lượng giá trị riêng biệt sau mỗi lần cập nhật. Sau khi áp dụng thao tác thứ k, chúng tôi quét toàn bộ mảng và chèn tất cả các giá trị vào một tập hợp để đếm số duy nhất. Điều này đúng vì một tập hợp sẽ loại bỏ các giá trị trùng lặp một cách tự nhiên. 

Tuy nhiên, mỗi lần quét tốn O(n) và chúng tôi thực hiện nó m lần, do đó độ phức tạp trên mỗi trường hợp kiểm thử sẽ trở thành O(nm). Với n và m lên tới 100000, điều này vượt xa giới hạn khả thi. 

Quan sát chính là chỉ có một phần tử thay đổi trong mỗi thao tác. Điều này có nghĩa là toàn bộ mảng không cần phải tính toán lại; chúng ta chỉ cần điều chỉnh sự đóng góp của một giá trị cũ và một giá trị mới. 

Nếu chúng tôi duy trì bản đồ tần số của các giá trị trên tất cả các bộ mã hóa thì mỗi lần cập nhật chỉ ảnh hưởng đến hai tần số: giá trị cũ của bộ mã hóa được cập nhật sẽ giảm đi một lần xuất hiện và giá trị mới tăng lên một lần xuất hiện. Số lượng giá trị riêng biệt chính xác là số lượng khóa trong bản đồ tần số có số dương này. 

Vì vậy, thay vì xây dựng lại tập hợp, chúng tôi duy trì số lượng tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Bảo trì bản đồ tần số | O((n + m) log n) hoặc O(n + m) trung bình | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc: một mảng lưu trữ các giá trị hiện tại cho mỗi bộ mã hóa và một bản đồ băm lưu trữ số lần mỗi giá trị xuất hiện trên toàn cầu. 

### Các bước 

1. Khởi tạo một mảng`a`với phần thưởng ban đầu của tất cả các lập trình viên. 
2. Xây dựng bản đồ tần suất`freq`Ở đâu`freq[x]`hiện có bao nhiêu lập trình viên có giá trị`x`. 
3. Tính số giá trị riêng biệt ban đầu là số lượng khóa trong`freq`với tần số khác 0. 
4. Đối với mỗi bản cập nhật`(p, r)`, xác định giá trị hiện tại`old = a[p]`. 
5. Giảm`freq[old]`bởi vì lập trình viên`p`sẽ không còn giá trị này nữa. 
6. Nếu`freq[old]`trở thành 0, xóa nó khỏi bản đồ hoặc coi nó là vắng mặt vì nó không còn đóng góp vào các giá trị riêng biệt. 
7. Tính giá trị mới`new = old + r`và gán nó cho người lập trình`p`. 
8. Tăng`freq[new]`bằng một, tạo mục nhập nếu cần thiết. 
9. Sau mỗi lần cập nhật, xuất số lượng khóa hiện tại vào`freq`. 

Mỗi bước được thúc đẩy bởi thực tế là chỉ có một phần tử thay đổi trong mỗi thao tác, do đó, việc cập nhật cấu trúc toàn cầu giảm xuống còn hai lần điều chỉnh ngược lại. 

### Tại sao nó hoạt động 

Điều bất biến chính là sau khi xử lý mỗi bản cập nhật, bản đồ tần số thể hiện chính xác tập hợp các giá trị phần thưởng hiện tại trên tất cả các lập trình viên. Vì các giá trị riêng biệt tương ứng chính xác với các giá trị có tần số lớn hơn 0 nên việc đếm các khóa hoạt động trong bản đồ sẽ mang lại câu trả lời đúng. Vì mỗi lần cập nhật chỉ thay đổi một vị trí mảng nên việc điều chỉnh hai số đếm sẽ duy trì tính bất biến này mà không cần tính toán lại toàn bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out_lines = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        freq = {}
        distinct = 0

        for x in a:
            if x not in freq:
                freq[x] = 0
            if freq[x] == 0:
                distinct += 1
            freq[x] += 1

        for _ in range(m):
            p, r = map(int, input().split())
            p -= 1

            old = a[p]

            freq[old] -= 1
            if freq[old] == 0:
                distinct -= 1

            new = old + r
            a[p] = new

            if new not in freq:
                freq[new] = 0
            if freq[new] == 0:
                distinct += 1
            freq[new] += 1

            out_lines.append(str(distinct))

    print("\n".join(out_lines))

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp bất biến. Mảng`a`lưu trữ các giá trị hiện tại để mỗi bản cập nhật có thể truy cập giá trị cũ trong O(1). Từ điển`freq`theo dõi sự đa dạng. Biến`distinct`tránh tính toán lại số lượng khóa hoạt động bằng cách duy trì số lượng tăng dần khi số đếm vượt qua 0 theo một trong hai hướng. 

Phải cẩn thận khi xử lý quá trình chuyển đổi. Khi giảm một giá trị, bộ đếm riêng biệt chỉ giảm khi số đếm của nó chính xác bằng 0. Khi tăng một giá trị, bộ đếm riêng biệt chỉ tăng khi nó chuyển từ 0 sang 1. Điều này tránh việc tính hai lần các giá trị đã tồn tại. 

Việc lập chỉ mục được điều chỉnh bằng cách trừ đi một từ`p`bởi vì đầu vào sử dụng lập chỉ mục dựa trên 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, m = 3
a = [20, 10, 25, 5, 3]
updates = (5,2), (2,-5), (4,20)
```Chúng tôi theo dõi`(freq, distinct)`. 

| Bước | Hoạt động | Trạng thái mảng | thay đổi tần số | khác biệt | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | [20,10,25,5,3] | tất cả 1 | 5 | 
| 1 | +2 lúc 5 | [20,10,25,5,5] | 3:1→0, 5:1→2 | 4 | 
| 2 | -5 lúc 2 | [20,5,25,5,5] | 10:1→0, 5:2→3 | 3 | 
| 3 | +20 lúc 4 | [20,5,25,25,5] | 5:3→2, 25:1→2 | 3 | 

Dấu vết này cho thấy các bản sao chỉ giảm số lượng khác biệt khi một giá trị biến mất hoàn toàn chứ không phải khi nó chỉ thay đổi tần số. 

### Ví dụ 2 

đầu vào:```
n = 1, m = 3
a = [100]
updates: (1,400), (1,1), (1,-100)
```| Bước | Giá trị | tần số | khác biệt | 
| --- | --- | --- | --- | 
| 0 | [100] | {100:1} | 1 | 
| 1 | 500 | {500:1} | 1 | 
| 2 | 501 | {501:1} | 1 | 
| 3 | 401 | {401:1} | 1 | 

Mặc dù giá trị thay đổi mỗi lần nhưng luôn có chính xác một phần tử, do đó số lượng giá trị riêng biệt không bao giờ thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Trung bình O(n + m) cho mỗi trường hợp thử nghiệm | Mỗi bản cập nhật thực hiện các hoạt động bản đồ băm theo thời gian không đổi | 
| Không gian | O(n + m) | Bản đồ tần số lưu trữ tối đa n + m giá trị riêng biệt theo thời gian | 

Tổng số n và m trên tất cả các trường hợp thử nghiệm được giới hạn bởi 200000, do đó giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. Mỗi bản cập nhật được xử lý trong thời gian khấu hao O(1) bằng cách sử dụng hàm băm, làm cho toàn bộ đầu vào tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys

    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m = map(int, input().split())
            a = list(map(int, input().split()))
            freq = {}
            distinct = 0
            for x in a:
                if freq.get(x, 0) == 0:
                    distinct += 1
                freq[x] = freq.get(x, 0) + 1

            for _ in range(m):
                p, r = map(int, input().split())
                p -= 1
                old = a[p]
                freq[old] -= 1
                if freq[old] == 0:
                    distinct -= 1
                new = old + r
                a[p] = new
                if freq.get(new, 0) == 0:
                    distinct += 1
                freq[new] = freq.get(new, 0) + 1
                out.append(str(distinct))

        return "\n".join(out)

    return solve()

# provided sample (trimmed formatting assumed)
assert run("""1
5 3
20 10 25 5 3
5 2
2 -5
4 20
""") == "4\n3\n3"

# all equal updates
assert run("""1
3 2
7 7 7
1 1
2 -1
""") == "2\n2"

# single element
assert run("""1
1 3
10
1 5
1 -2
1 -3
""") == "1\n1\n1"

# create and destroy distinct values
assert run("""1
2 4
1 2
1 1
2 -1
1 -2
2 3
""") == "2\n2\n1\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng đơn, cập nhật lặp lại | hằng số 1 | ổn định của trường hợp phần tử đơn | 
| tất cả các giá trị bằng nhau | sửa các chuyển tiếp khác biệt | xử lý các chuyển đổi từ 0 sang một | 
| sáp nhập xen kẽ | tạo/xóa động | tính đúng đắn của việc cập nhật ranh giới tần số | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi một giá trị biến mất hoàn toàn. Giả sử tất cả các lập trình viên hiện có giá trị 5 ngoại trừ một và giá trị cuối cùng đó được cập nhật. Tần số của 5 giảm xuống 0 và phải giảm bộ đếm riêng biệt đi đúng một. Thuật toán xử lý việc này vì bước giảm sẽ kiểm tra`freq[old] == 0`sau khi trừ và giảm`distinct`chỉ vào thời điểm đó. 

Một trường hợp khác là giới thiệu một giá trị đã tồn tại. Nếu người lập mã thay đổi từ 3 thành 7, nhưng 7 đã xuất hiện ở nơi khác thì số lượng riêng biệt không được tăng. Séc`if freq[new] == 0`đảm bảo rằng chúng tôi chỉ tăng khác biệt khi giá trị trước đó không có. 

Trường hợp thứ ba là cập nhật lặp lại trên cùng một chỉ mục. Mảng`a[p]`luôn lưu trữ giá trị hiện tại, vì vậy mỗi bản cập nhật sẽ trừ chính xác trạng thái trước đó trước khi áp dụng trạng thái tiếp theo. Điều này tránh tích lũy những đóng góp cũ từ các bản cập nhật trước đó.
