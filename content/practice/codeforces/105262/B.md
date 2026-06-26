---
title: "CF 105262B - Lập chỉ mục lại"
description: "Chúng ta được cấp một cuốn sách trong đó mỗi chương có hai thuộc tính: tựa đề duy nhất và số trang bắt đầu duy nhất. Trong một cuốn sách đúng, các chương sẽ được sắp xếp theo số trang bắt đầu tăng dần, vì đó là thứ tự đọc thực tế."
date: "2026-06-24T02:31:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105262
codeforces_index: "B"
codeforces_contest_name: "Game of Coders 3.0"
rating: 0
weight: 105262
solve_time_s: 49
verified: true
draft: false
---

[CF 105262B - Lập chỉ mục lại](https://codeforces.com/problemset/problem/105262/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cuốn sách trong đó mỗi chương có hai thuộc tính: tựa đề duy nhất và số trang bắt đầu duy nhất. Trong một cuốn sách đúng, các chương sẽ được sắp xếp theo số trang bắt đầu tăng dần, vì đó là thứ tự đọc thực tế. 

Tuy nhiên, mục lục được cung cấp bị xáo trộn. Điểm mấu chốt là bản thân các chương không có gì bị sai lệch. Mỗi chương vẫn trỏ đến trang bắt đầu chính xác và tất cả các chương đều xuất hiện đúng một lần. Chỉ có thứ tự của họ trong danh sách là sai. 

Đối với mỗi câu hỏi, chúng ta được biết tiêu đề của một chương mà Eddard vừa đọc xong. Chúng ta phải xác định chương nào đứng ngay sau chương đó theo đúng thứ tự theo trang bắt đầu. Nếu chương đã hoàn thành là chương cuối cùng theo thứ tự đọc, chúng ta sẽ xuất ra rằng không có chương tiếp theo. 

Kích thước đầu vào lớn: tổng cộng lên tới 10^5 chương và tối đa 10^5 truy vấn cho mỗi trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi giải pháp quét tất cả các chương cho mỗi truy vấn. Ngay cả O(nq) cũng sẽ đạt tới 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn 1 giây. Chúng tôi cần tiền xử lý cho phép mỗi truy vấn được trả lời trong thời gian không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi chương hoàn thành là chương có trang bắt đầu lớn nhất. Trong trường hợp đó, không có người kế thừa hợp lệ và chúng ta phải in một thông báo đặc biệt. Một trường hợp đặc biệt khác là khi các chương được đưa ra theo thứ tự hoàn toàn tùy ý, nghĩa là chúng ta không thể giả định bất kỳ thứ tự từng phần nào từ đầu vào. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng lại thứ tự đọc chính xác cho mọi truy vấn. Đối với mỗi truy vấn, chúng tôi có thể quét tất cả các chương, tìm trang của chương đó và sau đó quét lại để tìm trang lớn hơn tiếp theo. Điều này hoạt động hợp lý, bởi vì chương tiếp theo chỉ đơn giản là trang nhỏ nhất lớn hơn trang hiện tại. Tuy nhiên, mỗi truy vấn sẽ tốn O(n), cho tổng thời gian là O(nq), quá chậm khi cả n và q đều có thể là 10^5. 

Quan sát quan trọng là cấu trúc không bao giờ thay đổi. Thứ tự chính xác được xác định hoàn toàn bằng cách sắp xếp các chương theo trang bắt đầu của chúng. Khi chúng tôi sắp xếp chúng một lần, mỗi chương đều có phần kế tiếp cố định. Điều này biến vấn đề thành một vấn đề ánh xạ: đối với mỗi tiêu đề chương, chúng ta muốn biết tiêu đề tiếp theo trong danh sách đã sắp xếp. 

Chúng ta có thể tính toán trước một từ điển từ tiêu đề chương đến vị trí của nó trong mảng được sắp xếp. Sau đó, việc trả lời một truy vấn sẽ trở thành một quá trình tra cứu liên tục, sau đó là kiểm tra hàng xóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu (sắp xếp + bản đồ băm) | O(n log n + n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc tất cả các chương thành danh sách theo cặp (trang, tựa đề hoặc tựa đề, trang). Chúng tôi lưu trữ cả hai vì việc sắp xếp phải được thực hiện theo trang, nhưng các truy vấn lại theo tiêu đề. Sự tách biệt này là cần thiết vì các tiêu đề không được sắp xếp theo bất kỳ cách nào có ý nghĩa. 
2. Sắp xếp danh sách các chương theo trang bắt đầu theo thứ tự tăng dần. Điều này tái tạo lại thứ tự đọc thực sự của cuốn sách, vì các trang xác định chặt chẽ thứ tự và là duy nhất. 
3. Xây dựng một từ điển ánh xạ từng tiêu đề chương vào chỉ mục của nó trong danh sách được sắp xếp. Điều này cho phép chúng ta chuyển trực tiếp từ chuỗi truy vấn đến vị trí của nó theo thứ tự. 
4. Đối với mỗi tiêu đề truy vấn, hãy truy xuất chỉ mục của nó trong O(1) bằng từ điển. 
5. Nếu chỉ mục không phải là vị trí cuối cùng trong danh sách đã sắp xếp, hãy xuất tiêu đề ở chỉ mục + 1. Nếu không thì xuất ra "No More Chapters". 

Ý tưởng quan trọng là khi thứ tự được cố định trên toàn cầu, mỗi truy vấn sẽ trở thành tra cứu lân cận cục bộ chứ không phải là vấn đề tìm kiếm. 

### Tại sao nó hoạt động

Việc sắp xếp theo số trang sẽ tạo ra thứ tự tổng thể chính xác duy nhất của các chương vì số trang khác biệt và xác định thứ tự tổng thể nghiêm ngặt. Từ điển đảm bảo rằng mỗi tiêu đề chương được liên kết với chính xác một vị trí theo thứ tự này. Vì mối quan hệ kế tiếp trong một mảng được sắp xếp được xác định rõ ràng và ổn định nên câu trả lời cho mỗi truy vấn chính xác là phần tử tiếp theo trong chuỗi cố định này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, q = map(int, input().split())
        chapters = []

        for _ in range(n):
            s, p = input().split()
            p = int(p)
            chapters.append((p, s))

        chapters.sort()  # sort by page

        pos = {}
        for i, (p, s) in enumerate(chapters):
            pos[s] = i

        for _ in range(q):
            c = input().strip()
            i = pos[c]
            if i == n - 1:
                out.append("No More Chapters")
            else:
                out.append(chapters[i + 1][1])

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là xây dựng lại thứ tự chính xác bằng cách sắp xếp các chương trên trang bắt đầu của chúng. Từ điển`pos`là rất quan trọng vì nó loại bỏ việc quét lặp lại trong quá trình truy vấn. Mỗi truy vấn sẽ trở thành một tra cứu chỉ mục trực tiếp. 

Một lỗi triển khai phổ biến là quên loại bỏ chuỗi đầu vào cho các truy vấn, điều này có thể dẫn đến các khóa từ điển không khớp. Một trường hợp khác là vô tình sắp xếp theo tiêu đề thay vì trang do nhầm lẫn thứ tự bộ dữ liệu; đảm bảo tuple là`(page, title)`tránh điều này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
Chapter2 43
Chapter3 60
Chapter1 1
Chapter1
Chapter2
Chapter3
```Các chương được sắp xếp trở thành: 

| Bước | Chương (sắp xếp theo trang) | Truy vấn | Chỉ mục | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | (1, Chương 1), (43, Chương 2), (60, Chương 3) | Chương 1 | 0 | Chương 2 | 
| 2 | giống nhau | Chương 2 | 1 | Chương 3 | 
| 3 | giống nhau | Chương 3 | 2 | Không Còn Chương | 

Điều này xác nhận rằng một khi việc sắp xếp đã được cố định, các câu trả lời chỉ là những kiểm tra kề cận đơn giản. 

### Ví dụ 2 

đầu vào:```
3 1
SecondChapter 4
FirstChapter 1
ThirdChapter 24
FirstChapter
```Thứ tự sắp xếp: 

| Bước | Chương | Truy vấn | Chỉ mục | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | (1, Chương một), (4, Chương hai), (24, Chương ba) | Chương đầu tiên | 0 | Chương thứ hai | 

Điều này chứng tỏ rằng thứ tự đầu vào là không liên quan và chỉ có thứ tự trang mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q) | Sắp xếp chiếm ưu thế, mỗi truy vấn là O(1) | 
| Không gian | O(n) | Lưu trữ danh sách chương và bản đồ vị trí | 

Các ràng buộc cho phép tổng cộng tối đa 10^5 chương, do đó, bước tiền xử lý O(n log n) nằm trong giới hạn thoải mái. Việc xử lý truy vấn nói chung là tuyến tính theo q, điều này là tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, q = map(int, input().split())
        chapters = []

        for _ in range(n):
            s, p = input().split()
            chapters.append((int(p), s))

        chapters.sort()
        pos = {s: i for i, (_, s) in enumerate(chapters)}

        for _ in range(q):
            c = input().strip()
            i = pos[c]
            if i == n - 1:
                out.append("No More Chapters")
            else:
                out.append(chapters[i + 1][1])

    return "\n".join(out)

# provided sample-like test
assert run("""1
3 3
Chapter2 43
Chapter3 60
Chapter1 1
Chapter1
Chapter2
Chapter3
""") == """Chapter2
Chapter3
No More Chapters"""

# boundary: single chapter
assert run("""1
1 2
Only 10
Only
Only
""") == """No More Chapters
No More Chapters"""

# already sorted
assert run("""1
3 2
A 1
B 2
C 3
A
B
""") == """B
C"""

# reverse order input
assert run("""1
3 2
C 3
B 2
A 1
A
C
""") == """B
No More Chapters"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chương đơn | Không còn chương nào hai lần | xử lý phần tử cuối cùng | 
| đã được sắp xếp | B, C | chính xác khi đầu vào đã được đặt hàng | 
| thứ tự ngược lại | B, Không Còn Chương | sắp xếp lại đầy đủ tính đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chỉ có một chương. Sau khi sắp xếp, chương đó vừa là chương đầu vừa là chương cuối. Từ điển vẫn ánh xạ nó tới chỉ mục 0, và vì`n - 1 == 0`, mọi truy vấn đều trả về chính xác "No More Chapters". 

Một trường hợp đặc biệt khác là khi thứ tự đầu vào được sắp xếp ngược lại theo trang. Việc sắp xếp vẫn khôi phục thứ tự đúng vì số trang xác định thứ tự tổng hợp nghiêm ngặt. Logic kề vẫn hợp lệ vì phần kế tiếp luôn có chỉ số + 1 trong mảng được sắp xếp. 

Trường hợp cạnh cuối cùng là các truy vấn lặp lại cho cùng một chương. Vì chúng tôi không sửa đổi bất kỳ trạng thái nào nên mỗi truy vấn sẽ được giải quyết một cách độc lập thông qua cùng một thao tác tra cứu từ điển, đảm bảo kết quả đầu ra nhất quán bất kể sự lặp lại.
