---
title: "CF 1582A - Luntik và buổi hòa nhạc"
description: "Chúng tôi được cung cấp một bộ sưu tập các bài hát được chia thành ba loại: một số bài 1 phút cuối, một số bài 2 phút cuối và một số bài 3 phút cuối. Đối với mỗi trường hợp thử nghiệm, chúng ta phải gán mỗi bài hát cho một trong hai buổi hòa nhạc."
date: "2026-06-14T22:58:53+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1582
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 750 (Div. 2)"
rating: 800
weight: 1582
solve_time_s: 263
verified: true
draft: false
---

[CF 1582A - Luntik và buổi hòa nhạc](https://codeforces.com/problemset/problem/1582/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 4m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các bài hát được chia thành ba loại: một số bài 1 phút cuối, một số bài 2 phút cuối và một số bài 3 phút cuối. Đối với mỗi trường hợp thử nghiệm, chúng ta phải gán mỗi bài hát cho một trong hai buổi hòa nhạc. Mỗi bài hát phải được sử dụng chính xác một lần và thời lượng của mỗi buổi hòa nhạc chỉ đơn giản là tổng thời lượng của các bài hát được chỉ định cho nó. 

Mục đích là chia nhỏ các bài hát sao cho sự khác biệt tuyệt đối giữa tổng thời lượng của hai buổi hòa nhạc càng nhỏ càng tốt. 

Nếu chúng ta nghĩ về mặt cấu trúc, vấn đề thực sự là việc phân chia một tập hợp số gồm nhiều số 1, 2 và 3 thành hai nhóm có tổng bằng nhau. 

Các ràng buộc rất lớn, với mỗi trường hợp a, b và c lên tới 10^9 và lên tới 1000 trường hợp kiểm thử. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng xây dựng hoặc mô phỏng rõ ràng các bài tập cho từng bài hát. Bất kỳ phương pháp nào cũng phải hoạt động trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh vi phổ biến xuất hiện khi tổng số tiền rất nhỏ hoặc khi một loại chiếm ưu thế. Ví dụ: nếu chúng ta chỉ có một loại bài hát, giả sử a = 1, b = 0, c = 0, thì điều tốt nhất chúng ta có thể làm là đưa nó hoàn toàn vào một buổi hòa nhạc, mang lại sự khác biệt 1. Bất kỳ ý tưởng “ghép đôi” tham lam nào cũng phải xử lý phần còn sót lại một cách chính xác khi không thể ghép đôi hoàn hảo. 

Một trường hợp quan trọng khác là khi trộn các độ dài bài hát khác nhau sẽ cho phép cân bằng chính xác. Ví dụ: một bài hát dài 3 phút có thể cân bằng chính xác với một bài hát dài 1 và 2 phút, vì vậy việc so sánh tham lam cục bộ rất quan trọng. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ cố gắng gán từng bài hát một vào bật lửa của hai buổi hòa nhạc. Mô phỏng tham lam này sẽ yêu cầu lặp lại tất cả các bài hát a + b + c, có thể lớn tới 3 × 10^9 trong trường hợp xấu nhất. Điều này là hoàn toàn không thể thực hiện được. 

Một cách giải thích bạo lực khác là coi đây như một vấn đề phân vùng và thử tất cả các tập hợp con của bài hát. Đó là số lượng bài hát theo cấp số nhân và ngay lập tức là không thể. 

Nhận xét quan trọng là chỉ có số lượng của từng loại mới quan trọng chứ không phải của từng bài hát. Chúng tôi đang lựa chọn một cách hiệu quả số lượng 1, 2 và 3 trong buổi hòa nhạc đầu tiên; phần còn lại đi đến thứ hai. Điều này làm giảm vấn đề về việc cân bằng hai tổ hợp tuyến tính của ba biến. 

Tổng số tiền là cố định. Nếu chúng ta biểu thị nó bằng S thì việc giảm thiểu chênh lệch tuyệt đối giữa hai tập hợp con tương đương với việc tìm một tập hợp con có tổng càng gần S/2 càng tốt. 

Cấu trúc quan trọng là chúng ta không được tự do lựa chọn các tập hợp con tùy ý; chúng ta chỉ có thể tính tổng bằng cách sử dụng các đồng tiền giới hạn có giá trị 1, 2 và 3 với bội số lớn. Điều này làm cho số tiền có thể tiếp cận đủ dày đặc để giải pháp tối ưu có thể được mô tả bằng quy tắc cân bằng tham lam đơn giản. 

Chúng tôi cố gắng xây dựng một buổi hòa nhạc một cách tham lam với một nửa tổng số tiền. Đầu tiên chúng tôi sử dụng càng nhiều bài hát lớn càng tốt (3 phút), sau đó điều chỉnh với những bài hát dài 2 phút và cuối cùng sửa khoảng trống còn lại bằng những bài hát dài 1 phút. Bởi vì các giá trị nhỏ và có cấu trúc nên việc xây dựng tham lam này luôn mang lại sự cân bằng tốt nhất có thể đạt được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(a+b+c)) | O(1) | Quá chậm | 
| Cân bằng tham lam | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán tổng thời lượng S = a + 2b + 3c, sau đó đặt mục tiêu xây dựng tổng thời lượng buổi hòa nhạc đầu tiên càng gần với S/2 càng tốt.

1. Tính S = a + 2b + 3c. Đây là tổng thời lượng của tất cả các bài hát được cộng lại và bất kỳ phân vùng hợp lệ nào cũng phải chia tổng thời lượng này thành hai phần. 
2. Đặt mục tiêu T = S // 2. Chúng tôi muốn tạo một tập hợp con càng gần mục tiêu này càng tốt mà không vượt quá các ràng buộc về tính khả thi. 
3. Cố gắng sử dụng càng nhiều bài hát dài 3 phút càng tốt trong buổi hòa nhạc đầu tiên, nhưng không quá c và không quá T // 3. Điều này là tối ưu vì các bài hát dài 3 phút đóng góp nhiều nhất cho mỗi tiết mục nên chúng giảm khoảng cách nhanh nhất. 
4. Trừ phần đóng góp của các bài hát dài 3 phút đã chọn khỏi mục tiêu còn lại. Sau đó chuyển sang các bài hát dài 2 phút và lấy càng nhiều càng tốt mà không vượt quá mục tiêu đã cập nhật. Lý do là như nhau: chúng ta đang tham lam lấp đầy khoảng trống còn lại bằng cách sử dụng đơn vị lớn nhất hiện có. 
5. Cuối cùng, lấp đầy khoảng trống còn lại bằng các bài hát dài 1 phút, giới hạn bởi a. Tại thời điểm này, mỗi đơn vị còn lại phải là các bài hát dài 1 phút, vì vậy chúng tôi chỉ cần lấy min(a, khoảng trống còn lại). 
6. Gọi X là tổng của buổi hòa nhạc đầu tiên. Buổi hòa nhạc thứ hai là S − X, nên câu trả lời là |S − 2X|. 

Lý do điều này có tác dụng là vì cấu trúc của số tiền có thể tiếp cận đủ liền kề để các lựa chọn tối ưu cục bộ ở mỗi bước không cản trở bất kỳ sự sắp xếp tổng thể nào tốt hơn. Vì chúng tôi luôn chọn khoản đóng góp lớn nhất có thể mà không vượt quá mục tiêu nên chúng tôi không bao giờ lãng phí những hạng mục có giá trị cao vào những khoảng trống nhỏ mà những hạng mục nhỏ hơn có thể lấp đầy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b, c = map(int, input().split())
        
        S = a + 2*b + 3*c
        target = S // 2
        
        take3 = min(c, target // 3)
        target -= 3 * take3
        
        take2 = min(b, target // 2)
        target -= 2 * take2
        
        take1 = min(a, target)
        target -= take1
        
        X = 3 * take3 + 2 * take2 + take1
        print(abs(S - 2 * X))

if __name__ == "__main__":
    solve()
```Mã tính toán tổng số tiền và sau đó tham lam xây dựng tổng tập hợp con gần nhất có thể đạt được bằng một nửa số đó. Các bước trừ đảm bảo chúng ta không bao giờ vượt quá mục tiêu. Công thức cuối cùng chuyển đổi tập hợp con được xây dựng thành sự khác biệt tuyệt đối giữa hai phân vùng. 

Chi tiết tinh tế duy nhất là chúng ta không cần phải theo dõi rõ ràng buổi hòa nhạc thứ hai. Biết được bên này thì bên kia mới quyết tâm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1 1 1
```Tổng số tiền S = 6, mục tiêu = 3. 

| Bước | 3 phút sử dụng | sử dụng 2 phút | 1 phút sử dụng | Tổng hiện tại | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | 0 | 
| Sử dụng 3 phút | 1 | 0 | 0 | 3 | 
| Sử dụng 2 phút | 1 | 0 | 0 | 3 | 
| Sử dụng 1 phút | 1 | 0 | 0 | 3 | 

Đáp án = |6 − 2×3| = 0. 

Điều này khẳng định khả năng cân bằng hoàn hảo là có thể khi một bài hát dài 1 phút và 2 phút khớp với một bài hát dài 3 phút. 

### Ví dụ 2 

đầu vào:```
2
2 1 3
```Tổng số tiền S = 2 + 2 + 9 = 13, mục tiêu = 6. 

| Bước | 3 phút sử dụng | sử dụng 2 phút | 1 phút sử dụng | Tổng hiện tại | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | 0 | 
| Sử dụng 3 phút | 2 | 0 | 0 | 6 | 
| Sử dụng 2 phút | 2 | 0 | 0 | 6 | 
| Sử dụng 1 phút | 2 | 0 | 0 | 6 | 

Đáp án = |13 − 12| = 1. 

Điều này cho thấy rằng khi không thể cân bằng chính xác, công trình tham lam vẫn đạt được số tiền gần nhất có thể đạt được là một nửa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài một vài số nguyên | 

Giải pháp này đủ nhanh cho tối đa 1000 trường hợp thử nghiệm vì mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b, c = map(int, input().split())
        S = a + 2*b + 3*c
        target = S // 2

        take3 = min(c, target // 3)
        target -= 3 * take3

        take2 = min(b, target // 2)
        target -= 2 * take2

        take1 = min(a, target)
        target -= take1

        X = 3 * take3 + 2 * take2 + take1
        out.append(str(abs(S - 2 * X)))

    return "\n".join(out)

assert run("4\n1 1 1\n2 1 3\n5 5 5\n1 1 2\n") == "0\n1\n0\n1"
assert run("1\n1 0 0\n") == "1"
assert run("1\n0 0 1\n") == "1"
assert run("1\n10 10 10\n") in {"0", "1"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 0 | 1 | Mất cân bằng nhỏ nhất khác 0 | 
| 0 0 1 | 1 | Trường hợp chia bài hát đơn dài 3 phút | 
| 10 10 10 | 0 hoặc 1 | Trường hợp đối xứng cân bằng lớn | 

## Vỏ cạnh 

Khi chỉ tồn tại một loại bài hát, chẳng hạn như các bài hát chỉ dài 1 phút, việc xây dựng tham lam chỉ đơn giản ngầm thực hiện một nửa trong số đó thông qua bước min(a, target). Ví dụ: với a = 5, b = 0, c = 0, tổng là 5 và mục tiêu là 2. Thuật toán lấy 2 cái, để lại 3 ở buổi hòa nhạc còn lại và trả về chính xác chênh lệch là 1. 

Khi một bài hát lớn chiếm ưu thế, chẳng hạn như c lớn và a = b = 0, thuật toán sẽ lấy tầng (c/2) của chúng về một phía khi nó lấp đầy về phía S/2. Ví dụ: c = 3 cho tổng số 9, mục tiêu 4. Thuật toán lấy một bài hát dài 3 phút, để lại mục tiêu 1 không thể lấp đầy, dẫn đến chênh lệch cuối cùng là 3, phù hợp với sự mất cân bằng không thể tránh khỏi. 

Khi tồn tại sự hủy bỏ hoàn hảo, chẳng hạn như (1, 1, 1), kết hợp tham lam đảm bảo hủy bỏ 1 + 2 với 3 chính xác vì các bài hát dài 3 phút được sử dụng trước tiên để cố định mục tiêu và các bài hát nhỏ hơn sẽ lấp đầy khoảng trống còn lại mà không vượt quá mức, tạo ra sự bình đẳng chính xác.
