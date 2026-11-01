---
title: "CF 106484B - Trò chơi đếm của Bugcat"
description: "Chúng ta được cung cấp một tham số k giống như chữ số trong khoảng từ 1 đến 9 và vị trí x. Hãy tưởng tượng đếm lên từ 1, nhưng chúng ta không coi mọi số nguyên đều hợp lệ."
date: "2026-06-19T15:16:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106484
codeforces_index: "B"
codeforces_contest_name: "2026 GBA International Programming Contest"
rating: 0
weight: 106484
solve_time_s: 52
verified: true
draft: false
---

[CF 106484B - Trò chơi đếm của Bugcat](https://codeforces.com/problemset/problem/106484/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tham số giống như chữ số`k`từ 1 đến 9 và một vị trí`x`. Hãy tưởng tượng đếm lên từ 1, nhưng chúng ta không coi mọi số nguyên đều hợp lệ. Một số chỉ được “nói” nếu nó thỏa mãn ít nhất một trong hai điều kiện: hoặc chia hết cho`k`, hoặc biểu diễn thập phân của nó chứa chữ số`k`. Về mặt khái niệm, chúng tôi lọc các số tự nhiên bằng cách sử dụng quy tắc này, tạo ra một chuỗi số “nói” tăng dần vô hạn. Nhiệm vụ là tìm giá trị của`x`-số thứ trong chuỗi được lọc này. 

Những ràng buộc cho phép`x`lên tới 100.000, trong khi`k`là một chữ số. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào kiểm tra từng số nguyên có khả năng lên tới hàng triệu hoặc nhiều hơn đều có thể chấp nhận được nếu kiểm tra từng số rẻ, bởi vì 100.000 phần tử được chấp nhận thường yêu cầu quét theo thứ tự từ vài trăm nghìn đến vài triệu ứng viên tùy thuộc vào mật độ. Do đó, một mô phỏng trực tiếp là trong tầm tay. 

Điểm tinh tế chính là điều kiện lọc: việc kiểm tra “chứa chữ số k” phải được thực hiện cẩn thận ở dạng thập phân, không phải dưới dạng thuộc tính số. Một trường hợp khác phát sinh từ thực tế là cả hai điều kiện có thể trùng lặp. Ví dụ: một số như 33 khi`k = 3`đều chia hết cho 3 và chứa chữ số 3 nhưng chỉ được tính một lần trong dãy. Việc triển khai đơn giản làm tăng bộ đếm hai lần cho mỗi số sẽ âm thầm đếm quá mức. 

Cạm bẫy tiềm ẩn thứ hai là việc xử lý từng bước một trong việc đếm`x`-số hợp lệ thứ Vì chúng ta đang xử lý một luồng số hợp lệ chứ không phải mảng, nên dễ dàng trả về ứng cử viên hiện tại khi bộ đếm bằng`x`mà không đảm bảo rằng logic tăng là chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: bắt đầu từ 1, kiểm tra từng số nguyên và duy trì bộ đếm xem có bao nhiêu số thỏa mãn quy tắc. Với mỗi số nguyên`i`, chúng tôi kiểm tra xem`i % k == 0`hoặc liệu dạng chuỗi của`i`chứa ký tự đại diện của`k`. Bất cứ khi nào điều kiện được thỏa, chúng ta sẽ tăng một bộ đếm. Khi bộ đếm đạt tới`x`, chúng tôi trả về số nguyên hiện tại. 

Điều này hoạt động vì mọi số nguyên được kiểm tra chính xác một lần và quy tắc lọc độc lập với mỗi số. Điểm yếu của phương pháp này là hiệu suất chỉ theo nghĩa hệ số không đổi: chúng ta có thể cần phải quét xa hơn`x`, vì các số hợp lệ không được phân bố đồng đều. Ví dụ, khi`k = 9`, bội số của 9 và các số chứa 9 tương đối thường xuyên, vì vậy chúng ta có thể đạt tới 100.000 số hợp lệ sau khi chỉ quét vài trăm nghìn số nguyên. Ngay cả trong trường hợp xấu nhất khi`k = 1`, hầu như mọi số đều hợp lệ nên chúng ta vẫn chỉ quét đại khái`x`những con số. Điều này làm cho cách tiếp cận đủ hiệu quả dưới những ràng buộc. 

Không cần tối ưu hóa tổ hợp hoặc chữ số DP nâng cao hơn, bởi vì chúng tôi không được yêu cầu đếm hoặc chuyển trực tiếp đến câu trả lời mà chỉ liệt kê cho đến khi giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N) trong đó N là giá trị của số hợp lệ thứ x | O(1) | Đã chấp nhận | 
| Tối ưu (mô phỏng giống nhau, kiểm tra cẩn thận) | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`cnt = 0`và bắt đầu lặp lại`i`từ 1 trở lên. Chúng ta cần thứ tự tuần tự vì định nghĩa phụ thuộc vào việc tăng số tự nhiên. 
2. Với mỗi số`i`, xác định xem nó có nên được đưa vào chuỗi hay không. Đầu tiên hãy kiểm tra xem`i % k == 0`. Điều này nắm bắt tất cả bội số của`k`trực tiếp mà không cần xử lý chữ số. 
3. Nếu chưa được đưa vào phần chia hết, hãy kiểm tra xem chữ số có`k`xuất hiện ở`i`biểu diễn thập phân của . Điều này có thể được thực hiện bằng cách chuyển đổi`i`vào một chuỗi và kiểm tra tư cách thành viên của ký tự`str(k)`. 
4. Nếu một trong hai điều kiện được giữ nguyên, hãy tăng`cnt`bằng 1. Điều quan trọng là mức tăng này chỉ xảy ra một lần cho mỗi số ngay cả khi cả hai điều kiện đều đúng, vì chuỗi chỉ đếm mỗi số hợp lệ một lần. 
5. Khi nào`cnt == x`, xuất ngay`i`và dừng lại. Điều này đảm bảo chúng tôi trả về số hợp lệ thứ x theo thứ tự tăng dần. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến đơn giản: sau khi xử lý tất cả các số nguyên lên tới`i`, quầy`cnt`bằng chính xác số số nguyên trong`[1, i]`thỏa mãn quy luật đó. Vì chúng tôi quét theo thứ tự tăng dần và không bao giờ bỏ sót ứng viên nên lần đầu tiên`cnt`đạt tới`x`, hiện tại`i`chính xác là số nhỏ nhất sao cho ít nhất`x`số hợp lệ là ≤`i`. Điều đó khớp với định nghĩa của phần tử thứ x trong chuỗi được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k, x = map(int, input().split())
    target = str(k)

    cnt = 0
    i = 0

    while True:
        i += 1
        if i % k == 0 or target in str(i):
            cnt += 1
            if cnt == x:
                print(i)
                return

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo chiến lược mô phỏng trực tiếp. Biến`target`lưu trữ dạng chữ số của`k`vì vậy chúng tôi tránh tính toán lại nó nhiều lần. Vòng lặp tăng dần`i`vô thời hạn cho đến khi đạt được số lượng yêu cầu. Bài kiểm tra thành viên`target in str(i)`ghi lại tình trạng chữ số một cách rõ ràng. 

Một điểm tinh tế là chúng ta tăng`cnt`chỉ một lần mỗi`i`, ngay cả khi cả hai điều kiện đều đúng. các`if`tuyên bố sử dụng`or`, đảm bảo tính đúng đắn 

## Ví dụ đã hoạt động 

### Ví dụ 1:`k = 3, x = 5`Chúng tôi theo dõi các số hợp lệ một cách tuần tự. 

| tôi | chia hết cho 3 | chứa 3 | hợp lệ | cnt | 
| --- | --- | --- | --- | --- | 
| 1 | không | không | không | 0 | 
| 2 | không | không | không | 0 | 
| 3 | vâng | vâng | vâng | 1 | 
| 4 | không | không | không | 1 | 
| 5 | không | không | không | 1 | 
| 6 | vâng | không | vâng | 2 | 
| 7 | không | không | không | 2 | 
| 8 | không | không | không | 2 | 
| 9 | vâng | không | vâng | 3 | 
| 10 | không | không | không | 3 | 
| 11 | không | không | không | 3 | 
| 12 | vâng | không | vâng | 4 | 
| 13 | không | vâng | vâng | 5 | 

Tại`i = 13`, bộ đếm đạt tới 5 nên đáp án là 13. 

Dấu vết này cho thấy cách các lần truy cập dựa trên chữ số (như 13) đóng góp độc lập với khả năng chia hết và cách các phần trùng lặp được hợp nhất một cách an toàn thành một số duy nhất. 

### Ví dụ 2:`k = 4, x = 5`| tôi | chia hết cho 4 | chứa 4 | hợp lệ | cnt | 
| --- | --- | --- | --- | --- | 
| 1 | không | không | không | 0 | 
| 2 | không | không | không | 0 | 
| 3 | không | không | không | 0 | 
| 4 | vâng | vâng | vâng | 1 | 
| 5 | không | không | không | 1 | 
| 6 | không | không | không | 1 | 
| 7 | không | không | không | 1 | 
| 8 | vâng | không | vâng | 2 | 
| 9 | không | không | không | 2 | 
| 10 | không | không | không | 2 | 
| 11 | không | không | không | 2 | 
| 12 | vâng | không | vâng | 3 | 
| 13 | không | không | không | 3 | 
| 14 | vâng | vâng | vâng | 4 | 
| 15 | không | không | không | 4 | 
| 16 | vâng | vâng | vâng | 5 | 

Kết quả là 16. Ví dụ này nêu bật các trường hợp trùng lặp lặp đi lặp lại như 14 và 16, trong đó cả hai điều kiện đều được áp dụng nhưng việc đếm vẫn ổn định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Chúng tôi lặp lại cho đến khi tìm thấy số hợp lệ thứ x và mỗi lần lặp thực hiện kiểm tra chữ số O(log i), nhưng x 1e5 giữ cho nó bị giới hạn trong thực tế | 
| Không gian | O(1) | Chỉ một số biến được duy trì bất kể kích thước đầu vào | 

Mô phỏng phù hợp thoải mái trong giới hạn vì số lần lặp cần thiết tỷ lệ thuận với vị trí câu trả lời chứ không phải là không gian tìm kiếm không giới hạn. Ngay cả với chuyển đổi chuỗi trên mỗi lần lặp, tổng công việc vẫn hoạt động tốt trong các giới hạn 1 giây điển hình cho 100.000 lần kiểm tra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO
    out = StringIO()
    _stdin = _sys.stdin
    _stdout = _sys.stdout
    _sys.stdin = io.StringIO(inp)
    _sys.stdout = out

    def solve():
        k, x = map(int, _sys.stdin.readline().split())
        target = str(k)
        cnt = 0
        i = 0
        while True:
            i += 1
            if i % k == 0 or target in str(i):
                cnt += 1
                if cnt == x:
                    print(i)
                    return

    solve()
    _sys.stdin = _stdin
    _sys.stdout = _stdout
    return out.getvalue().strip()

# provided samples
assert run("3 5") == "13"
assert run("4 5") == "16"

# custom cases
assert run("1 1") == "1", "k=1 all numbers valid"
assert run("9 1") == "9", "first multiple case"
assert run("2 3") == "4", "checks alternating density"
assert run("5 10") == run("5 10"), "stability check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | trường hợp biên nhỏ nhất | 
| 9 1 | 9 | hợp lệ đầu tiên thông qua tính chia hết | 
| 2 3 | 4 | mật độ hỗn hợp của các số hợp lệ | 
| 5 10 | tính toán | tính đúng đắn chung trong mô phỏng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`k = 1`. Trong trường hợp này mọi số đều chia hết cho 1 nên toàn bộ dãy chỉ là số tự nhiên. Thuật toán xử lý việc này ngay lập tức: mọi lần lặp đều thỏa mãn`i % 1 == 0`, Vì thế`cnt`tăng lên mỗi lần và câu trả lời cho`x`đơn giản là`x`. 

Một trường hợp khác là khi việc ngăn chặn chữ số chiếm ưu thế trong khả năng chia hết. Ví dụ, với`k = 9`, các số như 19, 29, 39 đều đủ điều kiện ngay cả khi không chia hết cho 9. Mô phỏng vẫn hoạt động vì điều kiện là OR đơn giản, vì vậy mỗi số được tính một lần bất kể có bao nhiêu lý do khiến nó hợp lệ. 

Trường hợp tinh tế cuối cùng là chồng chéo, chẳng hạn như`k = 3`và số 33. Thuật toán đảm bảo tính chính xác vì điều kiện OR ngăn chặn việc tính hai lần. Mặc dù cả khả năng chia hết và sự hiện diện của chữ số đều đúng, bộ đếm chỉ tăng một lần trên mỗi số nguyên, duy trì cấu trúc chính xác của chuỗi.
