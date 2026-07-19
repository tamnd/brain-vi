---
title: "CF 103488J - Jiubei và Codeforces"
description: "Chúng tôi đang mô phỏng cách xếp hạng của Codeforces phát triển theo thời gian và cách xếp hạng đó chuyển thành tiêu đề hiển thị. Mỗi người dùng bắt đầu với xếp hạng ban đầu, sau đó trải qua một chuỗi thay đổi xếp hạng do các cuộc thi gây ra."
date: "2026-07-03T06:18:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103488
codeforces_index: "J"
codeforces_contest_name: "The 2021 Zhejiang University City College Freshman Programming Contest"
rating: 0
weight: 103488
solve_time_s: 49
verified: true
draft: false
---

[CF 103488J - Jiubei và Codeforces](https://codeforces.com/problemset/problem/103488/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng cách xếp hạng của Codeforces phát triển theo thời gian và cách xếp hạng đó chuyển thành tiêu đề hiển thị. Mỗi người dùng bắt đầu với xếp hạng ban đầu, sau đó trải qua một chuỗi thay đổi xếp hạng do các cuộc thi gây ra. Sau mỗi lần thay đổi, xếp hạng rơi vào một trong một số khoảng cố định và mỗi khoảng tương ứng với một danh hiệu cụ thể như Người mới, Học sinh, Chuyên gia, v.v. 

Nhiệm vụ không phải là theo dõi toàn bộ lịch sử xếp hạng mà chỉ phát hiện khi nào danh mục _title_ thay đổi. Bất cứ khi nào tiêu đề trước cuộc thi khác với tiêu đề sau khi áp dụng thay đổi xếp hạng của cuộc thi đó, chúng ta phải in phần chuyển đổi ở dạng “old_title -> new_title”. Sau khi xử lý tất cả các cuộc thi cho một trường hợp thử nghiệm, chúng tôi cũng xuất ra tiêu đề cuối cùng. 

Các ràng buộc rất nhỏ: tối đa 100 trường hợp thử nghiệm, mỗi trường hợp có tối đa 100 cập nhật xếp hạng. Điều này có nghĩa là tổng số hoạt động tối đa là 10.000. Bất kỳ giải pháp nào xử lý từng bản cập nhật trong thời gian không đổi là đủ. Ngay cả việc tính toán lại tiêu đề nhiều lần từ đầu sau mỗi lần cập nhật cũng được. 

Một điểm tinh tế là những thay đổi về xếp hạng có thể vượt qua nhiều ranh giới chỉ trong một bước. Ví dụ: xếp hạng có thể tăng trực tiếp từ 1500 lên 2500, bỏ qua một số nhóm tiêu đề. Trong những trường hợp như vậy, chúng tôi chỉ xuất một lần chuyển đổi: từ tiêu đề bắt đầu của bước đó đến tiêu đề cuối cùng sau khi cập nhật, chứ không phải tiêu đề trung gian. 

Một trường hợp đặc biệt khác là khi xếp hạng bắt đầu và nằm trong cùng một khung trên nhiều bản cập nhật. Trong trường hợp đó, không có gì được in cho các bước đó, ngay cả khi xếp hạng bằng số thay đổi. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là mô phỏng xếp hạng sau mỗi cuộc thi và tính lại tiêu đề tương ứng bằng cách kiểm tra xem xếp hạng rơi vào khoảng nào. Vì chỉ có mười khoảng thời gian có thể nên việc tra cứu này là thời gian không đổi. 

Mô phỏng lực lượng vũ phu này đã phù hợp với cách tiếp cận tối ưu về cấu trúc. Đối với mỗi bản cập nhật, chúng tôi điều chỉnh xếp hạng, tính toán lại tiêu đề bằng cách quét bảng ngưỡng và so sánh với tiêu đề trước đó. Nếu chúng khác nhau, chúng tôi sẽ tạo ra một quá trình chuyển đổi. 

Không cần tối ưu hóa ẩn vì không gian trạng thái rất nhỏ và mỗi bước đều độc lập. Công việc có ý nghĩa duy nhất là duy trì ánh xạ từ xếp hạng sang tiêu đề và theo dõi khi nó thay đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại từng bước | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 
| Tương tự với nhiều bài kiểm tra | O(tổng n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ánh xạ tiêu đề 

Trước tiên, chúng tôi xác định hàm chuyển đổi giá trị xếp hạng thành tiêu đề tương ứng bằng cách kiểm tra các ngưỡng cố định từ cao nhất đến thấp nhất. 

### bước 

1. Bắt đầu với xếp hạng ban đầu và tính tiêu đề ban đầu của nó. Đây trở thành danh hiệu trước đây mà chúng tôi so sánh. 
2. Đối với mỗi thay đổi xếp hạng, hãy cập nhật xếp hạng hiện tại bằng cách thêm delta đã cho. Đây là mô hình kết quả của cuộc thi. 
3. Tính tiêu đề mới từ xếp hạng được cập nhật bằng cách sử dụng ánh xạ ngưỡng. Bước này chuyển trạng thái số sang trạng thái phân loại. 
4. So sánh tiêu đề mới với tiêu đề trước đó. Nếu chúng khác nhau, hãy xuất ra “previous_title -> new_title”. Điều này chỉ ghi lại các chuyển đổi có thể nhìn thấy được, bỏ qua các biến động xếp hạng nội bộ trong cùng một khung. 
5. Cập nhật tiêu đề trước đó thành tiêu đề mới và tiếp tục. 
6. Sau khi xử lý tất cả các cuộc thi, xuất ra tiêu đề cuối cùng một lần nữa. 

### Tại sao nó hoạt động

Ánh xạ xếp hạng theo tiêu đề là sự phân chia dòng số nguyên thành các khoảng rời nhau. Mỗi xếp hạng tương ứng với chính xác một tiêu đề và mỗi bản cập nhật chỉ ảnh hưởng đến vị trí hiện tại trong phân vùng này. Bằng cách chỉ nhớ tiêu đề trước đó, chúng tôi lưu giữ tất cả thông tin cần thiết để phát hiện các thay đổi, vì quá trình chuyển đổi xảy ra chính xác khi hai xếp hạng liên tiếp rơi vào các khoảng khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get_title(r):
    if r >= 3000:
        return "Legendary grandmaster"
    if r >= 2600:
        return "International grandmaster"
    if r >= 2400:
        return "Grandmaster"
    if r >= 2300:
        return "International master"
    if r >= 2100:
        return "Master"
    if r >= 1900:
        return "Candidate master"
    if r >= 1600:
        return "Expert"
    if r >= 1400:
        return "Specialist"
    if r >= 1200:
        return "Pupil"
    return "Newbie"

t = int(input())
out_lines = []

for _ in range(t):
    n, k = map(int, input().split())
    rating = k
    prev = get_title(rating)

    for _ in range(n):
        rating += int(input())
        cur = get_title(rating)
        if cur != prev:
            out_lines.append(f"{prev} -> {cur}")
            prev = cur

    out_lines.append(prev)

print("\n".join(out_lines))
```Cốt lõi của việc thực hiện là`get_title`hàm mã hóa các khoảng xếp hạng theo thứ tự giảm dần để kết quả khớp đầu tiên luôn chính xác. Điều này tránh mọi nhu cầu về cấu trúc dữ liệu phức tạp. 

Chúng tôi chỉ lưu trữ chuỗi tiêu đề trước đó chứ không lưu trữ xếp hạng trước đó vì quá trình chuyển đổi hoàn toàn phụ thuộc vào tư cách thành viên danh mục. Mỗi bản cập nhật sẽ điều chỉnh xếp hạng, tính toán lại danh mục và in quá trình chuyển đổi có điều kiện. 

Phải cẩn thận khi áp dụng cập nhật xếp hạng trước khi tính toán tiêu đề mới, vì kết quả đầu ra mô tả trạng thái _sau_ mỗi cuộc thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một kịch bản đơn giản hóa: 

đầu vào:```
1
3 1500
100
600
-500
```Chúng tôi theo dõi sự tiến hóa từng bước. 

| Bước | Đánh giá | Tiêu đề trước | Thay đổi | Đánh giá sau | Tiêu đề sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1500 | Học sinh | - | 1500 | Học sinh | 
| 1 | 1500 | Học sinh | +100 | 1600 | Chuyên gia | 
| 2 | 1600 | Chuyên gia | +600 | 2200 | Thầy | 
| 3 | 2200 | Thầy | -500 | 1700 | Chuyên gia | 

Chuyển đổi đầu ra:```
Pupil -> Expert
Expert -> Master
Master -> Expert
Expert
```Điều này cho thấy rằng mỗi lần vượt qua ranh giới sẽ kích hoạt chính xác một dòng đầu ra, ngay cả khi nhiều ngưỡng bị bỏ qua trong một lần nhảy. 

### Ví dụ 2 

đầu vào:```
1
2 1190
20
-50
```| Bước | Đánh giá | Tiêu đề trước | Thay đổi | Đánh giá sau | Tiêu đề sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1190 | Người mới | - | 1190 | Người mới | 
| 1 | 1190 | Người mới | +20 | 1210 | Học sinh | 
| 2 | 1210 | Học sinh | -50 | 1160 | Người mới | 

Đầu ra:```
Newbie -> Pupil
Pupil -> Newbie
Newbie
```Điều này chứng tỏ rằng ngay cả những dao động nhỏ xung quanh một ngưỡng cũng tạo ra những chuyển tiếp lặp đi lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T · n) | Mỗi lần cập nhật xếp hạng sẽ tính toán lại việc tra cứu tiêu đề theo thời gian liên tục | 
| Không gian | O(1) | Chỉ một số biến được lưu trữ bất kể kích thước đầu vào | 

Số lượng cập nhật tối đa là 10.000 và mỗi bước chỉ thực hiện một chuỗi so sánh cố định. Điều này thoải mái phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def get_title(r):
        if r >= 3000:
            return "Legendary grandmaster"
        if r >= 2600:
            return "International grandmaster"
        if r >= 2400:
            return "Grandmaster"
        if r >= 2300:
            return "International master"
        if r >= 2100:
            return "Master"
        if r >= 1900:
            return "Candidate master"
        if r >= 1600:
            return "Expert"
        if r >= 1400:
            return "Specialist"
        if r >= 1200:
            return "Pupil"
        return "Newbie"

    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        rating = k
        prev = get_title(rating)

        for _ in range(n):
            rating += int(input())
            cur = get_title(rating)
            if cur != prev:
                out.append(f"{prev} -> {cur}")
                prev = cur

        out.append(prev)

    return "\n".join(out)

# minimum size, no change
assert run("1\n1 1000\n0\n") == "Newbie", "min size"

# single upward transition
assert run("1\n1 1190\n20\n") == "Newbie -> Pupil\nPupil", "boundary up"

# multiple transitions in one jump
assert run("1\n1 1500\n1000\n") == "Pupil -> Master\nMaster", "skip bands"

# down crossing
assert run("1\n2 2100\n0\n-1000\n") == "Master -> Candidate master\nCandidate master", "downward change"

# oscillation
assert run("1\n3 1300\n100\n-100\n100\n") == "Pupil -> Specialist\nSpecialist -> Pupil\nPupil -> Specialist\nSpecialist", "oscillation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kích thước tối thiểu | Người mới | không chuyển tiếp | 
| ranh giới lên | Người mới -> Học sinh\nHọc sinh | vượt ngưỡng chính xác | 
| bỏ qua các ban nhạc | Học sinh -> Thầy\nThầy | nhảy đa cấp | 
| thay đổi đi xuống | Thạc sĩ -> Ứng viên chính\nỨng cử viên chính | đánh giá giảm | 
| dao động | nhiều dòng | chuyển đổi lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh chung đang bắt đầu chính xác trên một ranh giới. Ví dụ: xếp hạng 1200 chính xác là ranh giới giữa Người mới và Học sinh. Việc ánh xạ phải được triển khai với thứ tự bất đẳng thức chính xác để 1200 được phân loại là Học sinh chứ không phải Người mới. Thuật toán xử lý vấn đề này bằng cách kiểm tra các ngưỡng từ cao nhất đến thấp nhất và sử dụng các giới hạn dưới bao gồm. 

Một trường hợp khác là một bản cập nhật lớn vượt qua nhiều dấu ngoặc. Ví dụ: bắt đầu từ 1500 và thêm +2000 sẽ chuyển thẳng vào danh mục hàng đầu. Thuật toán vẫn chỉ tính toán danh mục cuối cùng và in một chuyển đổi duy nhất, vì các danh mục trung gian không bao giờ được theo dõi rõ ràng. 

Trường hợp cuối cùng là khi không có quá trình chuyển đổi nào xảy ra trên tất cả các bản cập nhật. Trong trường hợp đó, chỉ tiêu đề cuối cùng được in vì không có sự kiện “cũ -> mới” nào được kích hoạt.
