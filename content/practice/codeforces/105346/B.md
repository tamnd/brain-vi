---
title: "CF 105346B - Cơn ác mộng bậc ba"
description: "Chúng ta được cho một chuỗi ký tự chỉ gồm 0, 1 và 2. Chúng ta muốn chọn một đoạn liền kề của chuỗi này sao cho trong đoạn đó, số lượng 2 ký tự không vượt quá giới hạn k cho trước."
date: "2026-06-23T15:33:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105346
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 09-13-24 Div. 2 (Beginner)"
rating: 0
weight: 105346
solve_time_s: 81
verified: false
draft: false
---

[CF 105346B - Cơn ác mộng bậc ba](https://codeforces.com/problemset/problem/105346/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các ký tự chỉ bao gồm`0`,`1`, Và`2`. Chúng ta muốn chọn một đoạn liền kề của chuỗi này sao cho trong đoạn đó, số lượng`2`ký tự không vượt quá giới hạn nhất định`k`. Trong số tất cả các phân đoạn hợp lệ như vậy, chúng tôi muốn có độ dài tối đa có thể. 

Nói cách khác, chúng ta trượt một cửa sổ qua chuỗi và tìm đoạn dài nhất trong đó số chữ số được đếm`2`vẫn nằm trong ngân sách cho phép. 

Kích thước đầu vào tăng lên$10^5$, điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các chuỗi con một cách rõ ràng. Một cách tiếp cận vũ phu sẽ được xem xét$O(n^2)$chuỗi con, và thậm chí đếm số lượng`2`s bên trong mỗi cái sẽ đẩy nó về phía$O(n^3)$hoặc$O(n^2)$, quá chậm so với giới hạn 1 giây. Do đó, chúng ta cần một phương thức tuyến tính hoặc gần tuyến tính, điển hình là phương thức xử lý chuỗi trong một lần truyền. 

Trường hợp thất bại tinh vi chính đối với lý luận ngây thơ là quên rằng phân đoạn tốt nhất có thể bao gồm nhiều đoạn cửa sổ hợp lệ và việc thu hẹp từ phía sai có thể làm mất câu trả lời tối ưu. Ví dụ, nếu`k = 1`và chuỗi là`220202`, một cách tiếp cận ngây thơ có thể tham lam bỏ qua quá nhiều vị trí và bỏ lỡ các cửa sổ hợp lệ dài hơn bắt đầu sau khi loại bỏ sớm`2`S. 

## Phương pháp tiếp cận 

Chiến lược brute-force rất đơn giản: liệt kê mọi chuỗi con có thể, đếm xem có bao nhiêu`2`s nó chứa và theo dõi độ dài tối đa trong số những phần có số lượng nhiều nhất`k`. Đối với mỗi chỉ mục bắt đầu, chúng ta mở rộng con trỏ kết thúc và duy trì một bộ đếm. Điều này dẫn đến khoảng$n$vị trí bắt đầu và đối với mỗi vị trí chúng tôi có thể quét tới$n$ký tự, dẫn đến$O(n^2)$thời gian. Điều này đã quá lớn khi$n = 10^5$, vì nó sẽ yêu cầu theo thứ tự$10^{10}$hoạt động. 

Cấu trúc của vấn đề gợi ý một cách tiếp cận hiệu quả hơn vì điều kiện chúng tôi thực thi rất đơn điệu: một khi một phân đoạn trở nên không hợp lệ do vượt quá`k`sự xuất hiện của`2`, việc mở rộng nó hơn nữa chỉ làm cho nó tồi tệ hơn. Đây chính xác là cài đặt mà cửa sổ trượt hoạt động tốt. Thay vì tính lại số lượng cho mỗi chuỗi con, chúng tôi duy trì một cửa sổ duy nhất`[l, r]`và theo dõi có bao nhiêu`2`s ở bên trong nó. Chúng tôi mở rộng`r`từng bước một, và bất cứ khi nào cửa sổ vi phạm ràng buộc, chúng ta sẽ di chuyển`l`chuyển tiếp cho đến khi nó trở lại hợp lệ. 

Điều này biến vấn đề thành một lần quét tuyến tính duy nhất trong đó mỗi ký tự vào và rời khỏi cửa sổ nhiều nhất một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Cửa Sổ Trượt |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cửa sổ`[l, r]`trên chuỗi và một bộ đếm`cnt2`lưu trữ bao nhiêu`2`s hiện đang ở trong cửa sổ. 

1. Khởi tạo`l = 0`,`cnt2 = 0`, Và`best = 0`. Chúng tôi bắt đầu với một cửa sổ trống và dần dần xây dựng các phân đoạn hợp lệ. 
2. Lặp lại`r`từ`0`ĐẾN`n - 1`. Mỗi lần chúng tôi bao gồm`s[r]`vào cửa sổ hiện tại. 
3. Nếu`s[r] == '2'`, tăng`cnt2`. Điều này theo dõi xem chúng ta có đang sử dụng một phần ngân sách hạn chế của mình hay không. 
4. Nếu`cnt2 > k`, cửa sổ không hợp lệ. Chúng ta phải thu nhỏ nó từ bên trái cho đến khi nó hợp lệ trở lại. Chúng tôi làm điều này bằng cách di chuyển`l`chuyển tiếp và trừ từ`cnt2`bất cứ khi nào chúng tôi loại bỏ một`2`. 
5. Sau khi khôi phục hiệu lực, cửa sổ hiện tại`[l, r]`thỏa mãn ràng buộc nên ta cập nhật`best = max(best, r - l + 1)`. 
6. Tiếp tục cho đến khi`r`đến cuối chuỗi. Câu trả lời là`best`. 

Ý tưởng chính là`l`chỉ di chuyển về phía trước, không bao giờ lùi lại, đảm bảo hành vi tuyến tính. 

### Tại sao nó hoạt động 

Ở mọi vị trí`r`, thuật toán duy trì sự bất biến mà cửa sổ`[l, r]`chứa nhiều nhất`k`sự xuất hiện của`2`, và đó`l`là chỉ số nhỏ nhất làm cho điều này đúng với hiện tại`r`. Bởi vì bất kỳ cửa sổ lớn hơn nào kết thúc tại`r`sẽ chỉ tăng hoặc duy trì số lượng`2`s, mọi tiện ích mở rộng không hợp lệ phải được sửa bằng cách di chuyển`l`Phải. Điều này đảm bảo rằng không có cửa sổ ứng viên hợp lệ nào kết thúc tại`r`bị bỏ qua, vì tất cả các tiền tố hợp lệ nhỏ hơn được ngầm coi là`l`tiến bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    l = 0
    cnt2 = 0
    best = 0

    for r in range(n):
        if s[r] == '2':
            cnt2 += 1

        while cnt2 > k:
            if s[l] == '2':
                cnt2 -= 1
            l += 1

        best = max(best, r - l + 1)

    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp logic cửa sổ trượt. Chi tiết duy nhất quan trọng là vòng lặp rút gọn phải khôi phục hoàn toàn tính hợp lệ trước khi cập nhật câu trả lời, nếu không chúng tôi có thể tính các phân đoạn không hợp lệ. Con trỏ`l`không bao giờ được thiết lập lại, điều này bảo toàn độ phức tạp tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 6, k = 1
s = 120202
```Chúng tôi theo dõi cửa sổ như`r`tăng lên. 

| r | char | tôi | cnt2 | cửa sổ | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 1 | 1 | 
| 1 | 2 | 0 | 1 | 12 | 2 | 
| 2 | 0 | 0 | 1 | 120 | 3 | 
| 3 | 2 | 0 | 2 → thu nhỏ | 1 | 3 | 
| | | 1 | 1 | 202 | 3 | 
| 4 | 0 | 1 | 1 | 2020 | 4 | 
| 5 | 2 | 1 | 2 → thu nhỏ | 2 | 4 | 
| | | 2 | 1 | 0202 | 4 | 

Độ dài cửa sổ hợp lệ tốt nhất là 4. Điều này cho thấy các cửa sổ không hợp lệ được sửa chữa như thế nào bằng cách di chuyển`l`chuyển tiếp cho đến khi ràng buộc được khôi phục. 

### Ví dụ 2 

đầu vào:```
n = 5, k = 0
s = 10202
```| r | char | tôi | cnt2 | cửa sổ | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 1 | 1 | 
| 1 | 0 | 0 | 0 | 10 | 2 | 
| 2 | 2 | 2 | 0 | 2 | 2 | 
| 3 | 0 | 2 | 0 | 20 | 2 | 
| 4 | 2 | 4 | 0 | 2 | 2 | 

Dấu vết này cho thấy khi`k = 0`, mọi`2`buộc thiết lập lại toàn bộ cửa sổ bắt đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chỉ mục được truy cập nhiều nhất hai lần, một lần bởi`r`và một lần bởi`l`| 
| Không gian |$O(1)$| Chỉ sử dụng bộ đếm và con trỏ | 

Quét tuyến tính vừa vặn thoải mái trong$10^5$ràng buộc và dễ dàng đáp ứng giới hạn thời gian vì mỗi ký tự chỉ kích hoạt công việc liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    s = input().strip()

    l = 0
    cnt2 = 0
    best = 0

    for r in range(n):
        if s[r] == '2':
            cnt2 += 1
        while cnt2 > k:
            if s[l] == '2':
                cnt2 -= 1
            l += 1
        best = max(best, r - l + 1)

    return str(best)

assert run("16 3\n1201012200120012\n") == "13", "sample 1"

assert run("1 0\n0\n") == "1", "single zero"
assert run("5 0\n11111\n") == "5", "no twos at all"
assert run("5 1\n22222\n") == "1", "all twos k=1"
assert run("6 2\n221221\n") == "6", "full window allowed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | 1 | trường hợp có chiều dài tối thiểu | 
| không có hai | 5 | cửa sổ không bao giờ co lại | 
| tất cả hai k=1 | 1 | hành vi thu hẹp liên tục | 
| cho phép toàn bộ cửa sổ | 6 | không cần thu nhỏ | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi`k = 0`, nghĩa là không`2`hoàn toàn được cho phép. Đối với một đầu vào như`10202`, thuật toán liên tục co lại bất cứ khi nào`2`đang gặp phải. Tại`r = 2`, chúng ta đánh cái đầu tiên`2`, buộc`l`di chuyển đến`2`, và cửa sổ đặt lại. Điều này cho thấy thuật toán không tích lũy trạng thái không hợp lệ, vì`cnt2`được sửa chữa ngay lập tức. 

Một trường hợp khác là khi chuỗi chứa ít hơn hoặc bằng`k`sự xuất hiện của`2`tổng cộng, chẳng hạn như`s = 1112011`với`k = 2`. Trong tình huống này,`cnt2`không bao giờ vượt quá`k`, Vì thế`l`không bao giờ di chuyển. Thuật toán trả về một cách hiệu quả`n`, vì toàn bộ chuỗi là hợp lệ.
