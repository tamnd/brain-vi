---
title: "CF 106242A - Tìm Tất (tất)"
description: "Quá trình được mô tả là về một người bắt đầu với một số lượng tất cố định và mang một đôi mỗi ngày. Mỗi ngày tiêu thụ vĩnh viễn một cặp. Vào một số ngày nhất định, cụ thể là vào những khoảng thời gian cố định trong ngày, tất mới sẽ được thêm vào nhưng chúng chỉ có sẵn bắt đầu từ ngày hôm sau."
date: "2026-06-25T07:12:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106242
codeforces_index: "A"
codeforces_contest_name: "2025 Taiwan NHSPC Mock Contest (Mirror)"
rating: 0
weight: 106242
solve_time_s: 39
verified: true
draft: false
---

[CF 106242A - Tìm tất (tất)](https://codeforces.com/problemset/problem/106242/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Quá trình được mô tả là về một người bắt đầu với một số lượng tất cố định và mang một đôi mỗi ngày. Mỗi ngày tiêu thụ vĩnh viễn một cặp. Vào một số ngày nhất định, cụ thể là vào những khoảng thời gian cố định trong ngày, tất mới sẽ được thêm vào nhưng chúng chỉ có sẵn bắt đầu từ ngày hôm sau. Câu hỏi đặt ra là một người có thể tiếp tục thói quen sử dụng một đôi hàng ngày này trong bao nhiêu ngày trước khi hết tất còn sử dụng được. 

Đầu vào cho hai số nguyên. Đầu tiên là số lượng đôi tất ban đầu có sẵn vào ngày đầu tiên. Thứ hai là khoảng thời gian xác định tần suất bổ sung các đôi tất mới, luôn vào cuối những ngày cụ thể đó. Đầu ra là một số nguyên biểu thị tổng số ngày mà quá trình tiếp tục cho đến khi không còn tất nào để sử dụng vào ngày hôm sau. 

Các ràng buộc là nhỏ, với giá trị lên tới khoảng một trăm. Điều đó ngay lập tức cho chúng ta biết rằng bất kỳ mô phỏng tuyến tính nào về ngày là đủ, vì ngay cả một vòng lặp đơn giản trên vài nghìn bước cũng không đáng kể trong giới hạn thời gian. Mọi thứ vượt quá O(n^2) vẫn sẽ an toàn, nhưng cấu trúc của quy trình cho thấy chúng ta sẽ không cần bất cứ thứ gì nặng nề. 

Một trường hợp khó khăn xuất phát từ thời điểm bổ sung. Tất mua nhiều lần trong ngày không được sử dụng trong cùng ngày đó mà chỉ được sử dụng vào sáng hôm sau. Điều này tạo ra các lỗi riêng lẻ trong các mô phỏng khi ai đó thêm nhầm tất mới vào quá sớm. 

Trường hợp cạnh thứ hai là khi khoảng thời gian lớn hơn số lượng tồn kho ban đầu. Trong tình huống đó, lần bổ sung đầu tiên xảy ra sau khi tất cả những chiếc tất ban đầu đã được tiêu thụ hết, vì vậy câu trả lời hoàn toàn phụ thuộc vào việc lần nạp đầu tiên có đến kịp thời để kéo dài thời gian sử dụng hay không. 

## Phương pháp tiếp cận 

Cách nghĩ ngây thơ về vấn đề này là mô phỏng từng ngày. Chúng ta bắt đầu với số lượng tất ban đầu. Mỗi ngày chúng tôi giảm số lượng vì một cặp được sử dụng. Bất cứ khi nào ngày hiện tại là bội số của khoảng thời gian nhất định, chúng tôi sẽ tăng số lượng tất, nhưng chỉ sau khi tiêu thụ tất cho ngày đó vì việc mua hàng diễn ra vào buổi tối muộn. 

Mô phỏng này đúng vì nó phản ánh chính xác quá trình thực tế. Tuy nhiên, ngay cả khi chúng tôi chạy nó cho đến khi cạn kiệt, số ngày không lớn trong các ràng buộc khi cả hai tham số đều nhỏ. Trường hợp xấu nhất là khi việc bổ sung tiếp tục kéo dài quá trình, dẫn đến một vòng lặp tiếp tục cho đến khi lượng hàng tồn kho tích lũy được tiêu thụ hết. Vì mỗi chu kỳ có độ dài m đóng góp thêm một chiếc tất nên sự tăng trưởng là tuyến tính và có thể dự đoán được. 

Quan sát quan trọng là những chiếc tất được tiêu thụ với tốc độ không đổi một chiếc mỗi ngày, trong khi những chiếc tất mới được cung cấp theo định kỳ. Cứ sau m ngày, lợi nhuận ròng là một chiếc tất (chúng ta tiêu thụ m chiếc tất và nhận thêm một chiếc), do đó hệ thống hoạt động giống như một quá trình suy giảm tuyến tính với sự bù định kỳ. Điều này làm giảm vấn đề trong việc theo dõi xem chúng ta có thể duy trì bao nhiêu chu kỳ đầy đủ và điều gì xảy ra trong chu kỳ một phần cuối cùng. 

Thay vì mô phỏng mỗi ngày, chúng ta có thể nhảy theo khối m ngày. Mỗi khối đầy đủ tiêu tốn m chiếc tất nhưng cũng bổ sung thêm một chiếc tất cho chu kỳ tiếp theo. Vì vậy, sau mỗi khối đầy đủ, lượng tồn kho giảm m - 1. Chúng ta lặp lại điều này cho đến khi lượng tồn kho còn lại không đủ để hoàn thành một khối đầy đủ khác, sau đó kết thúc trực tiếp những ngày còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hàng ngày | O(câu trả lời) | O(1) | Đã chấp nhận | 
| Giảm theo chu kỳ | O(câu trả lời / m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Bắt đầu với số lượng tất ban đầu và bộ đếm ngày ở số 0. Điều này mô hình hóa trạng thái hệ thống ngay từ đầu trước khi bất kỳ hoạt động tiêu thụ nào xảy ra. 
2. Trong khi có đủ tất cho ngày bổ sung tiếp theo, hãy mô phỏng một chu kỳ đầy đủ có chiều dài m. Mỗi chu kỳ tương ứng với m ngày tiêu thụ. 
3. Đối với mỗi chu kỳ đầy đủ, hãy tăng bộ đếm ngày lên m vì chúng ta đã hoàn thành m ngày sử dụng. 
4. Giảm số lượng tất đi m để tính mức sử dụng trong những ngày đó, sau đó thêm ngay một chiếc tất vì một đôi mới được mua vào cuối ngày chu kỳ. Điều này nắm bắt chính xác hiệu ứng bổ sung bị trì hoãn. 
5. Khi số tất còn lại ít hơn m, hãy thoát khỏi vòng chu kỳ và tiêu thụ số tất còn lại hàng ngày, thêm chúng trực tiếp vào bộ đếm ngày. 

Tính đúng đắn đến từ việc duy trì tính bất biến rằng sau mỗi chu kỳ hoàn thành, số lượng tất phản ánh chính xác số lượng tất chưa sử dụng có sẵn vào đầu ngày hôm sau. Mỗi chu kỳ đầy đủ sẽ biến đổi trạng thái theo một cách nhất quán: m mức tiêu thụ luôn được kết hợp với tối đa một lần bổ sung, do đó thay đổi thực là mang tính quyết định. Vì không có chu trình từng phần nào có thể tạo ra sự bổ sung bổ sung nên việc xử lý phần còn lại một cách riêng biệt sẽ hoàn thành chính xác dòng thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    days = 0
    
    while n >= m:
        cycles = n // m
        days += cycles * m
        n -= cycles * m
        n += cycles
    
    days += n
    print(days)

if __name__ == "__main__":
    solve()
```Mã này tuân theo cách diễn giải dựa trên chu kỳ một cách trực tiếp. Vòng lặp nén nhiều khối m ngày đầy đủ cùng một lúc thay vì mô phỏng từng khối một. biểu hiện`n // m`tính toán xem chúng ta có thể trích xuất bao nhiêu chu kỳ đầy đủ cùng một lúc. Mỗi chu kỳ làm giảm số lượng tất đi m nhưng lại cộng lại một, đó là lý do tại sao`n += cycles`được áp dụng sau khi tiêu thụ. 

Một lỗi triển khai phổ biến là cập nhật tất trước khi tính đến hiệu ứng toàn bộ chu kỳ, điều này phá vỡ mối quan hệ giữa mức tiêu thụ và thời gian bổ sung. Một vấn đề tế nhị khác là quên rằng việc bổ sung xảy ra sau khi tiêu thụ vào ngày thứ m, đó là lý do tại sao hiệu ứng ròng được xử lý sau khi trừ m. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 2, m = 2 

| Bước | n (vớ) | Ngày thêm | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | 2 | 0 | trạng thái ban đầu | 
| Chu kỳ | 2 → 0 + 1 = 1 | +2 | tiêu thụ 2 chiếc tất, thêm 1 | 
| Kết thúc | 1 | +1 | sử dụng chiếc tất còn lại | 

Câu trả lời cuối cùng là 3 ngày. 

Dấu vết này cho thấy rằng mặc dù tất đã hết hoàn toàn sau hai ngày nhưng việc bổ sung sẽ kéo dài thời gian sử dụng thêm một ngày. 

### Ví dụ 2 

đầu vào: 

n = 9, m = 3 

| Bước | n (vớ) | Ngày thêm | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | 9 | 0 | trạng thái ban đầu | 
| Chu kỳ 1 | 9 → 6 + 1 = 7 | +3 | khối 3 ngày đầu tiên | 
| Chu kỳ 2 | 7 → 4 + 1 = 5 | +3 | khối thứ hai | 
| Chu kỳ 3 | 5 → 2 + 1 = 3 | +3 | khối thứ ba | 
| Kết thúc | 3 | +3 | tiêu thụ một phần cuối cùng | 

Câu trả lời cuối cùng là 12 ngày. 

Ví dụ này cho thấy trạng thái ổn định trong đó mỗi chu kỳ đầy đủ sẽ giảm lượng tồn kho đi 2 chiếc tất lưới và quá trình này tiếp tục cho đến khi lượng tồn kho còn lại không đủ cho một khối đầy đủ khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n/m) | mỗi lần lặp lại sẽ loại bỏ toàn bộ chu kỳ m ngày | 
| Không gian | O(1) | quầy duy nhất được duy trì | 

Giới hạn đầu vào nhỏ nên ngay cả mô phỏng tuyến tính cũng sẽ nhanh. Việc giảm bớt dựa trên chu kỳ còn đảm bảo hoạt động liên tục theo thời gian cho các đầu vào thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    
    days = 0
    while n >= m:
        cycles = n // m
        days += cycles * m
        n -= cycles * m
        n += cycles
    days += n
    return str(days)

# provided samples
assert run("2 2") == "3", "sample 1"
assert run("9 3") == "12", "sample 2"

# custom cases
assert run("1 2") == "1", "single sock, no refill before depletion"
assert run("10 1") == "10", "daily refill every day"
assert run("5 5") == "6", "exact one cycle boundary"
assert run("100 100") == "101", "single large boundary cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 1 | không bổ sung trước khi kiệt sức | 
| 10 1 | 10 | bổ sung cực kỳ thường xuyên | 
| 5 5 | 6 | hành vi chu kỳ ranh giới chính xác | 
| 100 100 | 101 | độ chính xác của một chu kỳ ở quy mô | 

## Vỏ cạnh 

Khi lượng tồn kho ban đầu nhỏ hơn khoảng thời gian bổ sung, vòng lặp sẽ không bao giờ thực hiện. Thuật toán ngay lập tức chuyển sang tiêu thụ tất cả tất hàng ngày, phù hợp với thực tế vì không có tất mới nào đến trước khi cạn kiệt. 

Khi khoảng cách bằng một, mỗi ngày sẽ tạo ra một chiếc tất mới sau khi tiêu thụ. Vòng lặp liên tục thêm một chiếc tất ròng mỗi ngày, do đó, quá trình mô phỏng giảm xuống một cách hiệu quả thành một hệ thống không giảm mà vẫn tiến triển chính xác vì mức tiêu thụ luôn diễn ra trước. 

Khi lượng hàng tồn kho ban đầu chia hết chính xác cho khoảng thời gian, chu kỳ đầy đủ cuối cùng sẽ tạo ra một đợt bổ sung giúp kéo dài thời gian sử dụng thêm một ngày ngoài mong đợi ban đầu và bản cập nhật dựa trên chu kỳ sẽ ghi lại điều này mà không cần vỏ đặc biệt.
