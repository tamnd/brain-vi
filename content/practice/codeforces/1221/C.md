---
title: "CF 1221C - Đội ngũ hoàn hảo"
description: "Mỗi truy vấn mô tả một nhóm sinh viên được chia thành ba nhóm. Một số sinh viên là lập trình viên, một số là nhà toán học và một số không có chuyên môn gì cả. Một nhóm hợp lệ phải có chính xác ba học sinh và phải bao gồm ít nhất một lập trình viên và ít nhất một nhà toán học."
date: "2026-06-13T18:13:11+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math"]
categories: ["algorithms"]
codeforces_contest: 1221
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 73 (Rated for Div. 2)"
rating: 1200
weight: 1221
solve_time_s: 291
verified: true
draft: false
---

[CF 1221C - Đội ngũ hoàn hảo](https://codeforces.com/problemset/problem/1221/C) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, toán học 
**Thời gian giải:** 4 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi truy vấn mô tả một nhóm sinh viên được chia thành ba nhóm. Một số sinh viên là lập trình viên, một số là nhà toán học và một số không có chuyên môn gì cả. Một nhóm hợp lệ phải có chính xác ba học sinh và phải bao gồm ít nhất một lập trình viên và ít nhất một nhà toán học. Chỗ trống còn lại, nếu có, có thể được lấp đầy bởi bất kỳ sinh viên nào, kể cả những người không có chuyên môn hoặc thậm chí bằng cách sao chép loại vai trò giữa các lập trình viên hoặc nhà toán học miễn là đáp ứng được yêu cầu tối thiểu. 

Nhiệm vụ là xác định, đối với mỗi truy vấn, có bao nhiêu nhóm rời rạc cỡ ba có thể được thành lập theo những ràng buộc này. Mỗi học sinh có thể được sử dụng nhiều nhất một lần, vì vậy về cơ bản chúng tôi đang phân chia những người có sẵn thành các bộ ba hợp lệ. 

Các ràng buộc đi lên đến$10^8$mỗi danh mục và tối đa$10^4$truy vấn. Điều này ngay lập tức loại trừ mọi mô phỏng hoặc tìm kiếm trên các kết hợp. Bất kỳ giải pháp nào cũng phải giảm mỗi truy vấn thành số học có thời gian không đổi, vì$10^4$truy vấn thậm chí với$O(\sqrt{n})$hoặc$O(n)$mỗi truy vấn sẽ quá chậm. 

Một trường hợp thất bại tinh tế xuất hiện khi một nhóm rất lớn và những nhóm khác rất nhỏ. Ví dụ, nếu lập trình viên dồi dào nhưng nhà toán học lại khan hiếm, chúng ta không thể tham lam thành lập các nhóm sử dụng tất cả các lập trình viên trước vì các nhà toán học sẽ trở thành nút cổ chai. Tương tự, sinh viên “phổ thông” (không có chuyên ngành) linh hoạt nhưng không thể thay thế được yêu cầu mỗi đội phải có ít nhất một lập trình viên và một nhà toán học. 

Một sai lầm ngây thơ là cho rằng chúng ta nên luôn thành lập các nhóm một cách tham lam bằng cách sử dụng một lập trình viên và một nhà toán học trong mỗi nhóm và sau đó lấp đầy những sinh viên trung lập. Điều này hoạt động trong các trường hợp đơn giản nhưng không thành công khi cân bằng việc sử dụng nhóm trung lập có thể cho phép phân phối lại hiệu quả hơn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi liên tục cố gắng thành lập một nhóm bằng cách chọn một lập trình viên, một nhà toán học và một sinh viên bổ sung từ bất kỳ hạng mục nào. Chúng tôi tiếp tục cho đến khi chúng tôi không thể tạo thành một bộ ba hợp lệ. Điều này đúng vì mỗi bước đều tôn trọng các ràng buộc. Tuy nhiên, mỗi đội hình sẽ giảm số lượng đi một hoặc hai đơn vị tùy theo thành phần, vì vậy trong trường hợp xấu nhất chúng tôi thực hiện$O(c + m + x)$hoạt động cho mỗi truy vấn. Với giá trị lên đến$10^8$, điều này là không thể thực hiện được. 

Quan sát quan trọng là chỉ có hai ràng buộc thực sự quan trọng: mỗi nhóm sử dụng ít nhất một lập trình viên và ít nhất một nhà toán học. Các học sinh trung lập chỉ ảnh hưởng đến số lượng đội bổ sung có thể được hỗ trợ sau khi xác định được việc ghép đôi cốt lõi. 

Nếu trước tiên chúng ta ghép đôi các nhà lập trình và nhà toán học, số lượng các cặp bắt buộc như vậy bị giới hạn bởi$\min(c, m)$. Mỗi đội yêu cầu một lập trình viên và một nhà toán học, vì vậy nhiều nhất$\min(c, m)$các đội có thể tồn tại bất kể học sinh trung lập. Tuy nhiên, những học sinh trung lập có thể thay thế các vai trò còn thiếu một cách gián tiếp bằng cách lấp đầy vị trí thứ ba, điều này cho phép chuyển đổi các tình huống “mất cân bằng” thành các đội bổ sung vượt quá giới hạn ghép đôi trực tiếp. 

Thông tin chi tiết cốt lõi là mỗi nhóm có chính xác ba học sinh và mỗi nhóm hợp lệ phải bao gồm ít nhất một lập trình viên và một nhà toán học. Vì vậy, yếu tố hạn chế là số lượng người lập trình và nhà toán học cộng lại hoặc tổng số học sinh chia cho ba. Do đó, câu trả lời tối ưu bị giới hạn bởi cả hai:$$\min(c, m, \lfloor (c + m + x)/3 \rfloor)$$Biểu thức này nắm bắt cả hai ràng buộc: chúng tôi không thể vượt quá số lượng nhỏ hơn của các lập trình viên hoặc nhà toán học và chúng tôi không thể vượt quá tổng số nhóm có thể được hình thành từ tất cả học sinh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(c + m + x) mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị$c, m, x$cho một truy vấn. Chúng đại diện cho số lượng lập trình viên, nhà toán học và sinh viên trung lập. 
2. Tính tổng số học sinh$s = c + m + x$. Điều này thể hiện hạn chế về năng lực toàn cầu vì mỗi nhóm sử dụng ba học sinh. 
3. Tính toán$s // 3$, là số lượng nhóm tối đa có thể nếu chúng ta bỏ qua các ràng buộc về vai trò. Điều này đảm bảo chúng tôi không bao giờ vượt quá số học sinh sẵn có. 
4. Tính toán$\min(c, m)$, là số lượng đội tối đa có thể nếu mỗi đội phải sử dụng ít nhất một lập trình viên và một nhà toán học. Điều này thực thi tính khả thi của vai trò. 
5. Lấy mức tối thiểu của hai giới hạn này. Giao điểm này đảm bảo cả hai ràng buộc về nguồn lực đều được thỏa mãn đồng thời. 
6. Xuất kết quả cho truy vấn. 

### Tại sao nó hoạt động 

Mỗi đội có đúng ba học sinh riêng biệt nên tổng số đội không được vượt quá$s // 3$. Riêng mỗi đội cần ít nhất một lập trình viên và một nhà toán học nên số lượng đội không được vượt quá$c$hoặc$m$. Các học sinh trung lập chỉ ảnh hưởng đến cách lấp đầy ô thứ ba, nhưng chúng không thay đổi thực tế là mỗi đội phải rút thăm từ cả hai nhóm thiết yếu. Do đó, mức tối đa thực sự là giao điểm của các ràng buộc độc lập này, chính xác là mức tối thiểu của ba đại lượng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        c, m, x = map(int, input().split())
        total = c + m + x
        print(min(c, m, total // 3))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo công thức dẫn xuất trực tiếp. Tính toán duy nhất cho mỗi truy vấn là một vài phép tính số học, do đó nó chạy trong thời gian không đổi. Việc phân chia sử dụng phép chia sàn theo số nguyên, thực thi chính xác số lượng đội hoàn chỉnh tối đa. 

Một sai lầm phổ biến là cố gắng xây dựng các nhóm một cách tham lam bằng cách sử dụng các lập trình viên và nhà toán học trước tiên rồi điều chỉnh bằng những người trung lập. Cách tiếp cận đó có nguy cơ lạm dụng một danh mục theo cách ngăn chặn các đội hình tối ưu sau này. Giải pháp dạng đóng tránh mọi vấn đề về thứ tự bằng cách thu gọn toàn bộ quá trình ra quyết định thành các ràng buộc toàn cục. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 1 1
3 6 0
0 0 0
0 1 1
10 1 10
4 4 1
```Chúng tôi tính toán từng truy vấn như sau. 

| c | m | x | tổng cộng | tổng//3 | phút(c,m) | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 3 | 1 | 1 | 1 | 
| 3 | 6 | 0 | 9 | 3 | 3 | 3 | 
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 
| 0 | 1 | 1 | 2 | 0 | 0 | 0 | 
| 10 | 1 | 10 | 21 | 7 | 1 | 1 | 
| 4 | 4 | 1 | 9 | 3 | 4 | 3 | 

Bảng này cho thấy rằng trong mọi trường hợp, ràng buộc ràng buộc là tổng công suất hoặc nhỏ hơn đối với các lập trình viên và nhà toán học. Trong trường hợp thứ năm, mặc dù có tổng số học sinh nhiều nhưng chỉ có một nhà toán học nên chỉ có thể thành lập một đội hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi truy vấn được xử lý bằng các phép toán số học không đổi | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Giải pháp này sẽ chia tỷ lệ tuyến tính theo số lượng truy vấn, đây là giải pháp tối ưu vì tất cả thông tin đầu vào phải được đọc ít nhất một lần. Với$q \le 10^4$, điều này dễ dàng phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    q = int(input())
    out = []
    for _ in range(q):
        c, m, x = map(int, input().split())
        out.append(str(min(c, m, (c + m + x) // 3)))
    return "\n".join(out)

# provided samples
assert run("6\n1 1 1\n3 6 0\n0 0 0\n0 1 1\n10 1 10\n4 4 1\n") == "1\n3\n0\n0\n1\n3"

# custom cases
assert run("1\n0 5 10\n") == "0", "no coders"
assert run("1\n5 0 10\n") == "0", "no mathematicians"
assert run("1\n5 5 0\n") == "3", "balanced case"
assert run("1\n100 1 100\n") == "1", "single bottleneck"
assert run("1\n3 3 3\n") == "3", "perfect symmetry"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 5 10 | 0 | thiếu vai trò bắt buộc | 
| 5 0 10 | 0 | vai trò thiếu đối xứng | 
| 5 5 0 | 3 | giới hạn ghép đôi vai trò thuần túy | 
| 100 1 100 | 1 | sự thống trị nút cổ chai | 
| 3 3 3 | 3 | trường hợp tối ưu cân bằng hoàn toàn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một trong các vai trò bắt buộc bằng 0. Đối với đầu vào`0 10 10`, không có đội nào có thể được thành lập vì mỗi đội hợp lệ đều yêu cầu ít nhất một lập trình viên. Thuật toán tính toán`min(0, 10, 20//3)`mang lại kết quả chính xác`0`. 

Một trường hợp tế nhị khác là khi học sinh trung lập thì nhiều nhưng một vai lại rất nhỏ. Vì`10 1 100`, tổng là 111 nên tổng//3 là 37, nhưng min(c,m) là 1, nên đáp án là 1. Điều này khẳng định rằng học sinh trung lập không thể bù đắp cho việc thiếu các vai trò bắt buộc. 

Trường hợp cuối cùng là khi mọi thứ đã cân bằng, chẳng hạn như`3 3 3`. Ở đây tổng//3 bằng 3 và min(c,m) bằng 3, vì vậy tất cả học sinh có thể được phân chia hoàn hảo thành các nhóm hợp lệ có quy mô ba, điều này xác minh rằng công thức không giới hạn một cách giả tạo các đầu vào đối xứng.
