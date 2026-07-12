---
title: "CF 103186E - Zztrans \u7684\u5e84\u56ed"
description: "Chúng tôi đang mô phỏng một hệ thống đánh cá đơn giản hóa, trong đó mỗi lần ném tạo ra chính xác một con cá theo phân bố xác suất đã biết đối với các loại cá."
date: "2026-07-03T16:12:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103186
codeforces_index: "E"
codeforces_contest_name: "The 2021 Shanghai Collegiate Programming Contest"
rating: 0
weight: 103186
solve_time_s: 48
verified: true
draft: false
---

[CF 103186E - Zztrans \u7684\u5e84\u56ed](https://codeforces.com/problemset/problem/103186/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một hệ thống đánh cá đơn giản hóa, trong đó mỗi lần ném tạo ra chính xác một con cá theo phân bố xác suất đã biết đối với các loại cá. Mỗi loại cá thuộc một trong năm loại hiếm và mỗi loại có giá trị bán cố định, ngoại trừ loại hiếm nhất có giá trị khác nhau. 

Mỗi lần câu cá đều tiêu tốn một lượng mồi cố định, vì vậy mỗi lần câu sẽ tạo ra lợi nhuận ròng bằng giá trị của con cá đánh được trừ đi chi phí này. Người chơi thực hiện một số lần thử độc lập cố định và chúng tôi được yêu cầu tính toán tổng lợi nhuận dự kiến. 

Đầu vào đưa ra danh sách các loại cá, mỗi loại có nhãn độ hiếm và xác suất đánh bắt được. Các xác suất có tổng bằng 1, vì vậy mỗi lần ném luôn tạo ra chính xác một con cá được rút ra từ sự phân bổ này. 

Đầu ra là lợi nhuận dự kiến ​​sau k lần ném, trong đó lợi nhuận bao gồm cả giá trị bán cá và khoản khấu trừ chi phí mồi cho mỗi lần ném. 

Các ràng buộc n 100 và k 100 ngay lập tức chỉ ra rằng ngay cả các phương thức O(nk) hoặc O(n) trên mỗi trạng thái cũng dễ dàng đủ nhanh. Không cần mô phỏng hoặc lập trình động trên các trạng thái phức tạp, vì mỗi lần ép đều độc lập và được phân bổ giống hệt nhau. Điều này gợi ý rõ ràng rằng các thuộc tính kỳ vọng tuyến tính sẽ thu gọn toàn bộ vấn đề thành một giá trị kỳ vọng duy nhất trên mỗi lần truyền nhân với k. 

Một điểm tinh tế là danh mục “cá huyền thoại” sẽ ghi đè lên giá trị thông thường của nó. Mặc dù tất cả các xác suất đều độc lập và giống nhau, việc quên thay thế giá trị cho cá loại S sẽ tạo ra một kỳ vọng nhất quán nhưng không chính xác. 

Một cạm bẫy tiềm ẩn khác là lỗi tích lũy dấu phẩy động nếu người ta cố gắng mô phỏng k bước lặp đi lặp lại với phép tính tổng lặp đi lặp lại. Vì k nhỏ nên cả hai cách tiếp cận đều hoạt động, nhưng phép nhân lặp lại là không cần thiết và kém ổn định hơn so với việc tính toán một kỳ vọng duy nhất. 

## Phương pháp tiếp cận 

Một cách giải thích đơn giản về quy trình này là mô phỏng từng lần thử câu cá k một cách độc lập. Đối với mỗi lần thử, chúng tôi lấy mẫu cá theo cách phân phối nhất định, cộng giá trị của nó, trừ chi phí mồi và lặp lại. Giá trị kỳ vọng có thể xấp xỉ bằng mô phỏng Monte Carlo, nhưng điều đó sẽ gây ra tính ngẫu nhiên và sai sót, đồng thời không cần thiết với xác suất xác định. 

Cách tiếp cận kỳ vọng vũ phu mang tính xác định sẽ tính toán rõ ràng mức tăng dự kiến ​​​​của một lần ném bằng cách tính tổng tất cả các loại cá, nhân xác suất với giá trị và sau đó trừ đi chi phí mồi. Sau đó, người ta sẽ lặp lại phép tính này k lần hoặc nhân kỳ vọng của một lần thực hiện với k. 

Quan sát quan trọng là tính tuyến tính của kỳ vọng. Mỗi lần câu cá là độc lập và tổng phần thưởng là tổng của k biến ngẫu nhiên giống hệt nhau. Do đó, tổng phần thưởng dự kiến ​​chỉ gấp k lần phần thưởng dự kiến ​​của một lần sử dụng. Không cần phân phối chung hoặc theo dõi trạng thái. 

Điều này làm giảm toàn bộ vấn đề thành việc tính toán mức trung bình có trọng số duy nhất cho các giá trị cá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng | O(k · mẫu) | O(1) | Quá chậm/không chính xác | 
| Giá trị mong đợi cho mỗi lần diễn xuất | O(n + k) | O(1) | Đã chấp nhận | 
| Tối ưu hóa (nhân kỳ vọng) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Đọc tất cả các loại cá và xác suất của chúng, sau đó nhóm chúng theo bản đồ giá trị độ hiếm. 

Chúng ta cần dịch từng ký tự hiếm thành giá trị tiền tệ của nó. Quy tắc không chuẩn duy nhất là cá loại S sử dụng 10000 thay vì 100. 
2. Đối với mỗi loại cá, tính toán mức đóng góp của nó vào giá trị kỳ vọng bằng xác suất nhân với giá trị được chỉ định. 

Điều này trực tiếp xuất phát từ định nghĩa về kỳ vọng: mỗi kết quả đóng góp giá trị được tính theo khả năng xảy ra của nó. 
3. Tổng hợp tất cả các khoản đóng góp để đạt được giá trị kỳ vọng cho một lần câu cá.

Điều này thể hiện phần thưởng trung bình trước khi xem xét chi phí mồi. 
4. Trừ chi phí cố định của mồi (23) khỏi giá trị dự kiến ​​của một lần ném. 

Vì chi phí mồi được trả mỗi lần nên đây là phép trừ xác định và không tương tác với xác suất. 
5. Nhân kết quả kỳ vọng cho mỗi lần thực hiện với k. 

Tính tuyến tính của kỳ vọng đảm bảo rằng tổng phần thưởng mong đợi trên k lần diễn xuất độc lập chính xác là k lần kỳ vọng của một lần thực hiện. 

### Tại sao nó hoạt động 

Mỗi lần câu cá là một biến ngẫu nhiên độc lập có phân bố giống hệt nhau. Tổng lợi nhuận là tổng của k biến số như vậy, mỗi biến số đã bao gồm chi phí mồi xác định. Kỳ vọng phân phối trên tổng, vì vậy kỳ vọng của tổng số là tổng của kỳ vọng. Không có mối tương quan hoặc sự phụ thuộc có điều kiện nào tồn tại giữa các phiên bản, do đó không cần thêm trạng thái. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())
    
    value = {
        'D': 16,
        'C': 24,
        'B': 54,
        'A': 80,
        'S': 10000
    }
    
    exp_one = 0.0
    
    for _ in range(n):
        t, p = input().split()
        p = float(p)
        exp_one += p * value[t]
    
    exp_one -= 23.0
    ans = exp_one * k
    
    print(f"{ans:.4f}")

if __name__ == "__main__":
    main()
```Mã trực tiếp thực hiện tính toán kỳ vọng. Từ điển ánh xạ các lớp độ hiếm thành các giá trị, với phần ghi đè đặc biệt dành cho cá loại S. Mỗi con cá đóng góp giá trị xác suất lần vào một tổng số. 

Việc trừ 23 được thực hiện một lần cho mỗi lần ném chứ không phải cho mỗi con cá, vì chi phí mồi không phụ thuộc vào kết quả mà phụ thuộc vào số lần thử. Cuối cùng, nhân với k tổng hợp k phép thử độc lập giống hệt nhau. 

Một lỗi phổ biến là trừ chi phí mồi bên trong vòng lặp cho các loại cá, điều này sẽ làm tăng chi phí theo n thay vì k một cách không chính xác. 

## Ví dụ đã hoạt động 

Chúng tôi xây dựng một ví dụ nhỏ để minh họa tính toán. 

đầu vào:```
3 2
D 0.50
C 0.30
S 0.20
```Kỳ vọng từng bước: 

| Cá | Xác suất | Giá trị | Đóng góp | 
| --- | --- | --- | --- | 
| D | 0,50 | 16 | 8.0 | 
| C | 0,30 | 24 | 7.2 | 
| S | 0,20 | 10000 | 2000,0 | 

Tổng mỗi lần sử dụng = 2015,2 

Trừ mồi = 2015,2 − 23 = 1992,2 

Hai lần ném = 3984,4 

Điều này xác nhận tỷ lệ tuyến tính. 

Một ví dụ khác: 

đầu vào:```
2 3
D 1.00
S 0.00
```Giá trị mỗi lần truyền = 16 

Sau chi phí = -7 

Tổng cộng = -21 

Điều này cho thấy kỳ vọng có thể âm khi chi phí mồi vượt quá giá trị cá. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | vượt qua các loại cá để tính toán kỳ vọng | 
| Không gian | O(1) | chỉ cố định các biến ánh xạ và tích lũy | 

Các ràng buộc n 100 và k 100 làm cho điều này trở nên tầm thường về mặt hiệu suất. Ngay cả nhiều phép toán dấu phẩy động trên mỗi dòng đầu vào cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    value = {'D':16,'C':24,'B':54,'A':80,'S':10000}
    
    exp_one = 0.0
    for _ in range(n):
        t, p = input().split()
        p = float(p)
        exp_one += p * value[t]
    
    exp_one -= 23
    ans = exp_one * k
    return f"{ans:.4f}"

# provided sample (format reconstructed)
assert run("3 2\nD 0.50\nC 0.30\nS 0.20\n") == "3984.4000"

# minimum case
assert run("1 1\nD 1.00\n") == f"{(16-23):.4f}"

# all S fish
assert run("1 2\nS 1.00\n") == f"{(10000-23)*2:.4f}"

# mixed zero probability edge
assert run("2 3\nD 1.00\nS 0.00\n") == f"{(16-23)*3:.4f}"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn D | kỳ vọng tiêu cực | sự thống trị chi phí mồi | 
| tất cả S | nhân rộng giá trị lớn | ghi đè giá trị cao hiếm | 
| không có vấn đề S | danh mục bị bỏ qua | xác suất xử lý đúng đắn | 
| loại đơn | hành vi xác định | độ đúng cơ sở | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi xác suất gán trọng số bằng 0 cho cá có giá trị cao. Thuật toán vẫn đưa chúng vào ánh xạ, nhưng đóng góp của chúng bằng 0, vì vậy chúng không ảnh hưởng đến kỳ vọng. 

Ví dụ:```
2 2
S 0.00
D 1.00
```Kỳ vọng mỗi lần sử dụng là 16, trừ 23 sẽ là -7, tổng cộng là -14. Cá S hoàn toàn bị bỏ qua mặc dù có mặt. 

Một trường hợp khác là khi tất cả giá trị của cá đều thấp hơn giá mồi. Kỳ vọng trở nên âm, điều này hợp lý vì lợi nhuận kỳ vọng không nhất thiết phải dương.
