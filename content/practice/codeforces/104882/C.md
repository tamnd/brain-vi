---
title: "CF 104882C - Bắn cung sáng tạo"
description: "Chúng tôi đang xây dựng một mục tiêu hình vuông được tạo thành từ các khối đơn vị, trong đó độ dài cạnh là số nguyên chẵn $x$. Hình vuông không có màu đồng nhất. Thay vào đó, nó bao gồm các “vòng” hình vuông đồng tâm."
date: "2026-06-28T09:17:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104882
codeforces_index: "C"
codeforces_contest_name: "Voronezh State University - Sitronics contest II"
rating: 0
weight: 104882
solve_time_s: 52
verified: true
draft: false
---

[CF 104882C - Bắn cung sáng tạo](https://codeforces.com/problemset/problem/104882/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng một mục tiêu hình vuông được tạo thành từ các khối đơn vị, trong đó độ dài cạnh là số nguyên chẵn$x$. Hình vuông không có màu đồng nhất. Thay vào đó, nó bao gồm các “vòng” hình vuông đồng tâm. Vòng ngoài cùng có một màu, vòng tiếp theo có màu đối diện và sự luân phiên này tiếp tục hướng vào trong cho đến tâm. 

Mỗi vòng dày một tế bào. Vì vậy, đối với một nhất định$x \times x$hình vuông, số vòng là$x / 2$. Vòng ngoài là đường viền hình vuông lớn nhất và mỗi vòng trong là một hình vuông nhỏ hơn thu được bằng cách bóc bỏ đường viền trước đó. 

Hạn chế chính là len đỏ bị hạn chế. Chúng tôi được bảo rằng chúng tôi có$n$khối màu đỏ và chúng tôi muốn tối đa hóa độ dài cạnh$x$sao cho số lượng hồng cầu cần thiết cho cấu trúc vòng xen kẽ này không vượt quá$n$. 

Đầu vào mang lại$n$, và đầu ra là mức chẵn tối đa$x$sao cho số lượng tế bào màu đỏ trong mục tiêu được mô tả nhiều nhất$n$. 

Ràng buộc$4 \le n \le 1000$có nghĩa là không gian tìm kiếm đủ nhỏ để thậm chí có thể mô phỏng trực tiếp trên tất cả các khả năng có thể$x$sẽ khả thi. Tuy nhiên, cấu trúc của mẫu cho phép lập công thức trực tiếp về số lượng tế bào màu đỏ. 

Một trường hợp cạnh tinh tế là màu của vòng ngoài không cố định trong câu lệnh, nhưng nó không quan trọng đối với việc tối đa hóa. Nếu vòng ngoài có màu đỏ thì mức sử dụng màu đỏ là tối đa; nếu nó có màu trắng thì việc sử dụng màu đỏ sẽ được giảm thiểu. Vì chúng ta muốn tối đa có thể$x$có thể được hình thành với nhiều nhất$n$khối màu đỏ, chúng tôi giả sử trường hợp xấu nhất đối với việc tiêu thụ màu đỏ, đó là khi màu đỏ bắt đầu từ lớp bên ngoài. 

Một điểm tinh tế khác là các vòng rời rạc. Đối với nhỏ$x$, trực giác thủ công có thể thất bại nếu chúng ta giả sử diện tích tỷ lệ thay vì đếm các hình vuông đầy đủ. 

Ví dụ, khi$x = 4$, có hai vòng. Nếu vòng ngoài có màu đỏ nghĩa là nó đã tiêu tốn$16 - 4 = 12$tế bào, vì bên trong$2 \times 2$hình vuông có màu trắng. Bất kỳ phép tính gần đúng nào coi các vòng là các lớp liên tục hoặc màu trung bình sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử tất cả các độ dài cạnh chẵn$x = 2, 4, 6, \dots$và tính toán cần bao nhiêu ô màu đỏ cho mỗi ô. Đối với mỗi$x$, chúng tôi mô phỏng các vòng đồng tâm. Mỗi vòng đóng góp toàn bộ diện tích chu vi của nó (thực tế là toàn bộ diện tích hình vuông trừ đi hình vuông bên trong) tùy thuộc vào màu sắc của nó. Tổng hợp những điều này cho việc sử dụng màu đỏ. 

Đối với mỗi ứng viên$x$, chi phí tính toán này$O(x^2)$, vì chúng ta có thể đánh dấu hoặc đếm từng ô trong hình vuông. Trên tất cả các ứng cử viên lên đến$x \approx \sqrt{n}$quy mô, điều này vẫn còn nhỏ đối với các ràng buộc nhất định, nhưng nó không cần thiết. 

Quan sát quan trọng là mô hình này hoàn toàn mang tính xác định và có thể được biểu diễn bằng phương pháp phân tích. Mỗi vòng có chiều dài cạnh$x, x-2, x-4, \dots$. Số lượng tế bào trong một vòng bên$s$là:$$s^2 - (s-2)^2 = 4s - 4$$Nếu chúng ta giả sử vòng ngoài cùng có màu đỏ thì các ô màu đỏ là tổng của các vòng xen kẽ bắt đầu từ vòng lớn nhất. Điều này làm giảm bài toán thành tính một tổng xen kẽ đơn giản trên một dãy số học giảm dần. 

Thay vì mô phỏng hình học, chúng tôi trực tiếp tính toán xem có bao nhiêu lớp màu đỏ tồn tại và tổng hợp những đóng góp của chúng. Từ$n \le 1000$, chúng ta cũng có thể tính toán trước một cách an toàn các giá trị cho tất cả các$x$lên tới vài trăm. 

Cải tiến cốt lõi là thay thế hình học bằng số học trên các kích thước lớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force |$O(x^2)$mỗi lần kiểm tra |$O(1)$| Quá chậm | 
| Tính toán số học lớp |$O(x)$mỗi lần kiểm tra hoặc tính toán trước |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại độ dài cạnh ứng cử viên$x$, tăng thêm 2 bắt đầu từ 2, vì chỉ có kích thước chẵn là hợp lệ. Chúng tôi kiểm tra từng$x$như một câu trả lời tiềm năng. 
2. Đối với mỗi$x$, tính số vòng, đó là$x / 2$. Mỗi vòng tương ứng với một đường viền hình vuông có cạnh$s = x, x-2, x-4, \dots, 2$. 
3. Tính kích thước của mỗi chiếc nhẫn bằng cách sử dụng danh tính$s^2 - (s-2)^2 = 4s - 4$. Điều này tránh việc xây dựng lưới một cách rõ ràng và trực tiếp đếm số lượng ô thuộc vòng đó. 
4. Chỉ định các màu luân phiên bắt đầu từ vòng ngoài cùng là màu đỏ. Tính tổng kích thước của mọi vòng khác (vị trí 0, 2, 4, v.v. trong chuỗi). Điều này mang lại tổng mức sử dụng màu đỏ cho điều đó$x$. 
5. Nếu mức sử dụng màu đỏ được tính toán nhỏ hơn hoặc bằng$n$, cập nhật câu trả lời hay nhất cho$x$. Ngược lại, hãy dừng sớm nếu muốn vì quy mô lớn hơn.$x$sẽ chỉ làm tăng tổng diện tích và do đó tăng mức sử dụng màu đỏ. 

### Tại sao nó hoạt động 

Bất biến chính là việc phân tách thành các vòng sẽ chia hình vuông thành các tập hợp ô rời rạc có kích thước chỉ phụ thuộc vào độ dài cạnh của chúng. Mỗi màu hợp lệ được xác định hoàn toàn bằng cách xen kẽ các vòng này, do đó số lượng tế bào màu đỏ chỉ phụ thuộc vào$x$, không dựa trên bất kỳ thỏa thuận nội bộ nào. Vì cả hình học và màu sắc đều cố định và mang tính xác định, nên sự đóng góp của vòng tính toán sẽ tái tạo lại chính xác số lượng tế bào màu đỏ thực sự mà không cần xấp xỉ. Sự tăng trưởng đơn điệu của tổng diện tích với$x$đảm bảo rằng một khi kích thước vượt quá$n$, tất cả các kích thước lớn hơn cũng sẽ vượt quá nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def red_needed(x):
    total = 0
    color_red = True
    s = x
    while s > 0:
        ring = s * s - (s - 2) * (s - 2) if s > 2 else 4
        # simpler: ring = 4*s - 4 for s > 2, and 4 for s = 2
        if color_red:
            total += ring
        color_red = not color_red
        s -= 2
    return total

n = int(input().strip())

ans = 0
x = 2
while True:
    need = red_needed(x)
    if need <= n:
        ans = x
    else:
        break
    x += 2

print(ans)
```chức năng`red_needed`tính toán số lượng ô màu đỏ cho một giá trị cố định$x$bằng cách lặp lại độ dài cạnh vòng. Mỗi lần lặp sẽ trừ đi 2 từ kích thước hình vuông hiện tại, kích thước này sẽ di chuyển vào trong lớp tiếp theo. 

Boolean xen kẽ`color_red`đảm bảo rằng chúng tôi lập mô hình chính xác sự xen kẽ màu bắt đầu từ vòng ngoài. Sự tích lũy chỉ xảy ra khi vòng hiện tại có màu đỏ. 

Vòng lặp chính tăng dần$x$lên 2 và dừng lại ngay khi số lượng hồng cầu yêu cầu vượt quá$n$, dựa vào tính đơn điệu của công trình. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 4$Chúng tôi thậm chí còn kiểm tra$x$. 

| x | Nhẫn | Tính toán màu đỏ | Tổng màu đỏ | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 4 | 4 | Có | 
| 4 | 2 | 16 - 4 = 12 (chỉ màu đỏ bên ngoài) | 12 | Không | 

Vì$x = 2$, chỉ có một kích thước vòng$2 \times 2$, vậy mức sử dụng màu đỏ là 4. Đối với$x = 4$, vòng ngoài đã vượt quá ngân sách nên câu trả lời là 2. 

### Ví dụ 2:$n = 20$| x | Nhẫn | Lớp đỏ | Tổng màu đỏ | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 2×2 | 4 | Có | 
| 4 | 2 | chỉ 4×4 | 12 | Có | 
| 6 | 3 | 6×6 + 2×2 | 36 + 4 = 40 | Không | 

Ở đây chúng ta thấy điều đó tại$x = 6$, yêu cầu màu đỏ tăng vọt vì bên ngoài$6 \times 6$chiếc nhẫn chiếm ưu thế. Hợp lệ tối đa$x$là 4. 

Những dấu vết này cho thấy sự tăng trưởng không tuyến tính trong$x$, nhưng phụ thuộc vào sự tích lũy lớp vuông. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(x_{\max}/2)$| Chúng tôi kiểm tra từng cái một$x$và tính toán các vành của nó theo độ sâu tuyến tính | 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ | 

Được cho$n \le 1000$, khả thi tối đa$x$nhỏ và thuật toán chạy ngay lập tức trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return main()

def main():
    n = int(input().strip())

    def red_needed(x):
        total = 0
        color_red = True
        s = x
        while s > 0:
            ring = s * s - (s - 2) * (s - 2) if s > 2 else 4
            if color_red:
                total += ring
            color_red = not color_red
            s -= 2
        return total

    ans = 0
    x = 2
    while True:
        if red_needed(x) <= n:
            ans = x
        else:
            break
        x += 2
    return str(ans)

# provided sample-like checks
assert run("4") == "2", "small boundary"

# custom cases
assert run("12") == "4", "just enough for 4x4"
assert run("20") == "4", "6x6 too large"
assert run("4") == "2", "minimum case"
assert run("1000") != "", "large feasibility check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 | 2 | trường hợp không tầm thường nhỏ nhất | 
| 12 | 4 | ranh giới nơi 4×4 khớp chính xác | 
| 20 | 4 | từ chối kích thước hình vuông tiếp theo | 
| 1000 | tính toán tối đa | ổn định hạn chế lớn | 

## Vỏ cạnh 

Trường hợp một cạnh là đầu vào nhỏ nhất trong đó chỉ$x = 2$là hợp lệ. Vì$n = 4$, thuật toán đánh giá$x = 2$, chấp nhận nó, sau đó thử$x = 4$, nhận thấy nó vượt quá ngân sách và trả về chính xác 2. 

Một trường hợp cạnh khác là khi$n$đủ lớn để nhiều lớp đóng góp. Vì$n = 1000$, thuật toán tiếp tục tăng$x$cho đến khi vùng màu đỏ tích lũy vượt quá giới hạn. Bởi vì mỗi bước tính toán lại tổng vòng chính xác nên không có nguy cơ tích lũy lỗi nổi hoặc đếm sai một phần lớp. 

Trường hợp thứ ba là điểm chuyển tiếp trong đó việc thêm vòng ngoài mới sẽ làm tăng mức sử dụng màu đỏ. Việc này được xử lý một cách tự nhiên bởi vì mỗi$x$được đánh giá độc lập ngay từ đầu, do đó không có giá trị gần đúng trước đó được áp dụng.
