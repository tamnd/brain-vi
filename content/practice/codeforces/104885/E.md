---
title: "CF 104885E - \u0425\u043e\u0440\u043e\u0448\u0438\u0435-\u0445\u043e\u0440\u043e\u0448\u0438\u0435 \u043f\u043e\u0434\u043e\u0442\u0440\u0435\u0437\u043a\u0438"
description: "Chúng ta được cung cấp một mảng và chúng ta làm việc với các tổng tiền tố của nó. Đặt pref[i] biểu thị tổng của i phần tử đầu tiên, với pref[0] = 0. Một mảng con [l, r] được gọi là tốt khi tổng của nó bằng 0, tương đương với pref[r] - pref[l-1] = 0, hoặc pref[r] = pref[l-1]."
date: "2026-06-28T09:08:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104885
codeforces_index: "E"
codeforces_contest_name: "Municipal stage of ROI in Nizhny Novgorod 2023"
rating: 0
weight: 104885
solve_time_s: 41
verified: true
draft: false
---

[CF 104885E - \u0425\u043e\u0440\u043e\u0448\u0438\u0435-\u0445\u043e\u0440\u043e\u0448\u0438\u0435 \u043f\u043e\u0434\u043e\u0442\u0440\u0435\u0437\u043a\u0438](https://codeforces.com/problemset/problem/104885/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và chúng ta làm việc với các tổng tiền tố của nó. Cho phép`pref[i]`biểu thị tổng của số đầu tiên`i`các phần tử, với`pref[0] = 0`. Một mảng con`[l, r]`được gọi là tốt khi tổng của nó bằng 0, tương đương với`pref[r] - pref[l-1] = 0`, hoặc`pref[r] = pref[l-1]`. 

Sau đó, bài toán yêu cầu chúng ta đếm một khái niệm mạnh hơn: một mảng con được coi là tốt-tốt nếu nó chứa ít nhất một mảng con có tổng bằng 0. Xét về tổng tiền tố, điều này có nghĩa là bên trong phạm vi`[l-1, r]`, phải tồn tại hai chỉ số`i < j`như vậy`pref[i] = pref[j]`. Nếu tất cả tiền tố trong`[l-1, r]`là khác nhau thì không tồn tại mảng con có tổng bằng 0 và phân đoạn đó được coi là xấu-tốt. 

Vì vậy nhiệm vụ giảm xuống còn đếm các mảng con`[l, r]`sao cho tiền tố có tổng bằng`[l-1, r]`tất cả đều không khác biệt. 

Đầu vào là một mảng số nguyên và chúng ta phải tính số mảng con “tốt-tốt” như vậy. 

Nếu kích thước mảng lên tới khoảng`10^5`, bất kỳ phép liệt kê bậc hai nào của các mảng con đều trở nên không thể. Một sự ngây thơ`O(n^2)`quét đã tạo ra xung quanh`5 * 10^9`hoạt động quá chậm. Điều này ngay lập tức gợi ý rằng chúng ta cần quét tuyến tính hoặc gần tuyến tính, rất có thể là bằng cửa sổ trượt. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các tổng tiền tố là khác biệt trên toàn cầu. Ví dụ: nếu mảng tăng nghiêm ngặt các số dương như`[1, 2, 3]`, thì mọi tổng tiền tố đều khác biệt, do đó không có mảng con có tổng bằng 0 nào tồn tại ở bất kỳ đâu. Câu trả lời đúng phải là`0`. Một cách tiếp cận đơn giản chỉ kiểm tra các mảng con cục bộ mà không theo dõi tính duy nhất của tiền tố sẽ đếm không chính xác nhiều phân đoạn. 

Một trường hợp đặc biệt khác là khi mảng có nhiều tổng tiền tố lặp lại. Ví dụ`[1, -1, 1, -1]`tạo ra các tổng tiền tố lặp lại và nhiều mảng con có tổng bằng 0. Ở đây, việc đếm chính xác phụ thuộc vào việc phát hiện các ranh giới lặp lại đầu tiên chứ không phải liệt kê các mảng con. 

## Phương pháp tiếp cận 

Sửa lỗi chiến lược vũ phu`(l, r)`và kiểm tra xem có tồn tại tổng tiền tố trùng lặp trong`[l-1, r]`. Điều này yêu cầu quét phân đoạn hoặc sử dụng một bộ cho mỗi truy vấn. Mỗi lần kiểm tra là`O(n)`, và có`O(n^2)`phân đoạn, đưa ra`O(n^3)`hoặc`O(n^2 log n)`tùy theo việc thực hiện. Điều này là quá chậm. 

Quan sát quan trọng là lật điều kiện. Thay vì tính trực tiếp các phân đoạn tốt-tốt, chúng tôi xem xét phần bù: các phân đoạn trong đó tất cả các tổng tiền tố đều khác biệt. Đây chính xác là các phân đoạn không chứa tổng tiền tố lặp lại, nghĩa là không có phân đoạn tổng bằng 0 nào tồn tại bên trong chúng. 

Chúng tôi có thể sửa điểm cuối phù hợp`r`và duy trì phân đoạn hợp lệ dài nhất kết thúc tại`r`sao cho tổng tiền tố là duy nhất. Cho phép`l`là chỉ số nhỏ nhất sao cho`[l, r]`không hợp lệ, nghĩa là có tổng tiền tố lặp lại bên trong`[l, r]`. Khi đó mọi điểm xuất phát`l' < l`tạo ra các phân đoạn hợp lệ`[l', r]`đó là tốt-tốt, và tất cả`l' >= l`tạo ra những cái không hợp lệ. 

Như vậy, đối với mỗi`r`, sự đóng góp chính xác là`l`và chúng ta có thể tính toán nó bằng cách sử dụng cửa sổ trượt với bản đồ băm theo dõi lần xuất hiện cuối cùng của tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Cửa sổ trượt tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi mảng thành tổng tiền tố, vì vấn đề hoàn toàn là về sự bằng nhau của các giá trị tiền tố. 

Sau đó chúng tôi duy trì một cửa sổ trượt trên các chỉ số tiền tố`[l, r]`sao cho tất cả các tổng tiền tố bên trong đều khác biệt. 

1. Khởi tạo bản đồ băm`last`, lưu trữ chỉ mục cuối cùng nơi mỗi tổng tiền tố được nhìn thấy. Cũng được thiết lập`l = 0`Và`answer = 0`. 
2. Lặp lại`r`từ`0`ĐẾN`n`trên tổng tiền tố. Mỗi bước cố gắng mở rộng cửa sổ để bao gồm`pref[r]`. 
3. Nếu`pref[r]`đã được nhìn thấy trước đây ở vị trí`p`, Và`p >= l`, thì chúng ta phải di chuyển`l`ĐẾN`p + 1`. Điều này là cần thiết vì việc giữ`p`bên trong cửa sổ sẽ đưa ra tổng tiền tố trùng lặp, vi phạm tính duy nhất. 
4. Sau khi sửa chữa`l`, chúng tôi thêm sự đóng góp`l`để trả lời. Điều này đếm xem có bao nhiêu điểm bắt đầu tạo ra một đoạn xấu-tốt kết thúc ở`r`, tương ứng với các phân đoạn tốt-tốt trong công thức ban đầu. 
5. Cập nhật`last[pref[r]] = r`. 

Lý do chúng tôi sử dụng chỉ mục tiền tố thay vì chỉ mục mảng là vì tổng mảng con được mã hóa dưới dạng bằng nhau giữa hai vị trí tiền tố. 

### Tại sao nó hoạt động 

Tại bất kỳ điểm cố định nào`r`, cửa sổ`[l, r]`là hậu tố dài nhất kết thúc tại`r`với tất cả các tổng tiền tố riêng biệt. Bất kỳ điểm xuất phát nào`l' < l`đảm bảo rằng`[l', r]`phải chứa tổng tiền tố lặp lại, vì sự xuất hiện lặp lại tại`l-1`và một số vị trí trước đó bị buộc phải bên trong phân khúc. Ngược lại, bất kỳ`l' >= l`giữ tất cả các tổng tiền tố khác biệt bên trong phân khúc. Điều này tạo ra một phân vùng rõ ràng gồm các điểm bắt đầu hợp lệ và không hợp lệ, làm cho`l`ranh giới đóng góp chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    last = {}
    l = 0
    ans = 0

    for r in range(n + 1):
        x = pref[r]

        if x in last and last[x] >= l:
            l = last[x] + 1

        last[x] = r
        ans += l

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo ý tưởng cửa sổ trượt trên tổng tiền tố. Chi tiết quan trọng là chúng tôi lặp đi lặp lại`pref[0..n]`, không phải mảng ban đầu. Bản đồ`last`lưu trữ vị trí của tổng tiền tố và con trỏ`l`luôn nhảy về phía trước, không bao giờ lùi lại, đảm bảo độ phức tạp tuyến tính. 

Sự tích lũy`ans += l`tương ứng với việc đếm tất cả các vị trí bắt đầu hợp lệ cho các mảng con kết thúc tại mỗi`r`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 -1 1 -1
```Tổng tiền tố:`[0, 1, 0, 1, 0]`| r | trước[r] | bản đồ cuối cùng trước | tôi | hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | {} | 0 | chèn 0 | 0 | 
| 1 | 1 | {0:0} | 0 | chèn 1 | 0 | 
| 2 | 0 | {0:0,1:1} | 1 | nhân đôi 0 lực l=1 | 1 | 
| 3 | 1 | {...} | 2 | nhân đôi 1 lực l=2 | 3 | 
| 4 | 0 | {...} | 3 | nhân đôi 0 lực l=3 | 6 | 

Câu trả lời cuối cùng là`6`, cho thấy nhiều mảng con chứa các tổng tiền tố lặp lại, nghĩa là nhiều phân đoạn chứa mảng con có tổng bằng 0. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```Tổng tiền tố:`[0, 1, 3, 6]`tất cả đều khác biệt. 

| r | trước[r] | tôi | trả lời | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 1 | 1 | 0 | 0 | 
| 2 | 3 | 0 | 0 | 
| 3 | 6 | 0 | 0 | 

Không có bản sao nào xuất hiện, vì vậy`l`không bao giờ di chuyển và mọi phân đoạn đều không hợp lệ theo nghĩa ban đầu, đưa ra câu trả lời`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi chỉ mục tiền tố được xử lý một lần và mỗi lần cập nhật của`l`là đơn điệu | 
| Không gian | O(n) | lưu trữ bản đồ băm nhiều nhất`n`tổng tiền tố | 

Giải pháp phù hợp thoải mái trong giới hạn cho`n`lên tới`10^5`, vì nó chỉ thực hiện công tuyến tính và các phép toán băm theo thời gian không đổi trên mỗi bước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    last = {}
    l = 0
    ans = 0

    for r in range(n + 1):
        x = pref[r]
        if x in last and last[x] >= l:
            l = last[x] + 1
        last[x] = r
        ans += l

    return str(ans)

# all-positive distinct prefix sums
assert run("3\n1 2 3\n") == "0"

# alternating sum
assert run("4\n1 -1 1 -1\n") == "6"

# single element zero
assert run("1\n0\n") == "1"

# all zeros
assert run("3\n0 0 0\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n0\n`|`1`| hành vi trùng lặp tiền tố một phần tử | 
|`3\n0 0 0\n`|`6`| lực lặp lại nặng thường xuyên l ca | 
|`3\n1 2 3\n`|`0`| không có trường hợp va chạm tiền tố | 
|`4\n1 -1 1 -1\n`|`6`| mẫu lặp lại tiền tố xen kẽ | 

## Vỏ cạnh 

Khi tất cả các phần tử đều bằng 0, mỗi tổng tiền tố sẽ lặp lại ở mỗi bước. Thuật toán bắt đầu với`l = 0`, và mỗi cái mới`0`lực lượng`l`để chuyển đến lần xuất hiện cuối cùng cộng với một. Điều này tạo ra sự dịch chuyển tối đa và tạo ra sự đóng góp ngày càng tăng`0 + 1 + 2 + 3`. 

Khi tất cả các tổng tiền tố là khác nhau, bản đồ băm không bao giờ kích hoạt việc đặt lại`l`. Cửa sổ vẫn ở mức tối đa, nhưng vì không tồn tại tổng tiền tố lặp lại nên mọi phân đoạn đều không hợp lệ khi chứa mảng con có tổng bằng 0, dẫn đến đóng góp bằng 0. 

Khi một giá trị lặp lại cách xa nhau, chẳng hạn như tổng tiền tố`[0, 5, 10, 5, 15]`, nhảy vào`l`xảy ra chính xác khi lần thứ hai`5`xuất hiện. Con trỏ bỏ qua tất cả các lần khởi động không hợp lệ cùng một lúc, đây là cơ chế chính ngăn chặn hành vi bậc hai.
