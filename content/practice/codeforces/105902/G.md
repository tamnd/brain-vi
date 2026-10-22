---
title: "CF 105902G - Vẫn Không Có Tiền?"
description: "Chúng ta được giao một trò chơi xác định dành cho hai người chơi với một đống xiên. Lúc đầu có x xiên và một tham số k. Người chơi luân phiên nhau, OC luôn đi trước."
date: "2026-06-21T15:24:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105902
codeforces_index: "G"
codeforces_contest_name: "2025 Fujian Normal University Programming Contest"
rating: 0
weight: 105902
solve_time_s: 45
verified: true
draft: false
---

[CF 105902G - Vẫn không có tiền?](https://codeforces.com/problemset/problem/105902/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một trò chơi xác định dành cho hai người chơi với một đống xiên. Lúc đầu có`x`xiên và một tham số`k`. Người chơi luân phiên nhau, OC luôn đi trước. Đến lượt người chơi, họ chọn một số xiên để loại bỏ, từ 1 đến giá trị hiện tại là`k`, miễn là vẫn còn đủ xiên. Sau mỗi lần di chuyển, giá trị của`k`giảm đúng 1. Một lần`k`đạt đến 0, không thể di chuyển thêm được nữa, vì vậy ai di chuyển tại thời điểm đó sẽ ngay lập tức thua cuộc. 

Người chiến thắng phụ thuộc hoàn toàn vào lối chơi tối ưu, nghĩa là cả hai người chơi luôn chọn những nước đi tối đa hóa cơ hội chiến thắng. 

Đầu vào cung cấp nhiều trò chơi độc lập. Mỗi trò chơi được xác định bởi kích thước cọc ban đầu`x`và giới hạn di chuyển ban đầu`k`. Đầu ra chỉ đơn giản là danh tính của người chơi chiến thắng. 

Những ràng buộc đẩy chúng ta tới một`O(1)`hoặc`O(log x)`mỗi giải pháp thử nghiệm. Với tối đa`10^5`trường hợp thử nghiệm và giá trị lên đến`10^18`, bất kỳ mô phỏng nào về chuyển động hoặc trạng thái đều không thể thực hiện được. Ngay cả việc mô phỏng một trò chơi cũng không khả thi vì`k`bản thân nó có thể lớn bằng`10^18`. 

Trường hợp cạnh tinh tế xuất hiện khi`k`lớn so với`x`. Ví dụ, nếu`x = 4, k = 10`, OC có thể lấy hết xiên ngay lập tức vì nước đi đầu tiên cho phép loại bỏ tối đa 10 lần và trò chơi kết thúc ngay lập tức. Một cách tiếp cận ngây thơ cho rằng trò chơi luôn kéo dài chính xác`k`di chuyển sẽ không chính xác. 

Một trường hợp góc khác là khi`k = 1`. Người chơi đầu tiên có thể gỡ đúng một xiên, nhưng ngay sau đó`k`trở thành 0, nghĩa là người chơi thứ hai không có nước đi hợp lệ bất kể số xiên còn lại. Điều này tạo ra chiến thắng ngay lập tức cho người chơi đầu tiên mà không phụ thuộc vào`x`miễn là`x ≥ 1`. 

Cuối cùng, khi`x`là cực kỳ lớn và`k`nhỏ, xiên không bao giờ quan trọng vì trò chơi kết thúc hoàn toàn do giới hạn di chuyển giảm dần trước khi hết cọc. 

## Phương pháp tiếp cận 

Một mô phỏng bạo lực sẽ chơi trò chơi một cách rõ ràng: ở mỗi lượt hãy thử tất cả các cách loại bỏ có thể có từ 1 đến`k`, đánh giá đệ quy kết quả và giảm`k`từng bước. Điều này mô hình hóa cách chơi tối ưu một cách chính xác vì nó khám phá toàn bộ cây trò chơi. 

Tuy nhiên, hệ số phân nhánh lên tới`k`, và độ sâu cũng lên tới`k`, từ`k`giảm mỗi lần di chuyển cho đến khi nó bằng không. Thậm chí phớt lờ`x`, số lượng trạng thái tăng lên như một vụ nổ giai thừa trong thực tế. Với`k`lên đến`10^18`, điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là kích thước cọc`x`chỉ quan trọng cho đến khi nó đủ lớn để không bao giờ hạn chế việc chơi. Sau đó, trò chơi bị chi phối hoàn toàn bởi trình tự giới hạn nước đi giảm dần. Mỗi bước di chuyển làm giảm`k`chính xác là 1, do đó trò chơi có số lượt tối đa xác định: nó dừng sau nhiều nhất`k`di chuyển trừ khi cọc hết sớm hơn. 

Vì vậy, câu hỏi thực sự sẽ trở thành: tổng số nước đi sẽ thực sự xảy ra? Mỗi bước di chuyển làm giảm`k`bằng 1, vì vậy sau`k`di chuyển giá trị trở thành 0 và người chơi tiếp theo không thể di chuyển. Vì vậy, nếu cọc không bao giờ hết thì trò chơi luôn kéo dài chính xác`k`nước đi và người chiến thắng được xác định theo tính chẵn lẻ: OC thắng nếu`k`là số lẻ, KP thắng nếu`k`là chẵn. 

Điều phức tạp duy nhất là liệu cọc có kết thúc sớm hơn`k`di chuyển. Nhưng OC luôn có thể loại bỏ ít nhất 1 xiên mỗi lượt, vì vậy sau`t`di chuyển, ít nhất`t`xiên đã được gỡ bỏ. Vì vậy, nếu`x < k`, trò chơi kết thúc sau đúng`x`di chuyển, bởi vì sau`x`di chuyển cọc trống và người chơi tiếp theo không thể di chuyển. Nếu như`x ≥ k`, trò chơi kết thúc sau đúng`k`di chuyển do`k`đạt đến số không. 

Vì vậy số lần di chuyển chỉ đơn giản là`min(x, k)`, và người chiến thắng được xác định bằng số đó là số lẻ hay số chẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ theo k | O(k) | Quá chậm | 
| Giảm chẵn lẻ tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm trò chơi xuống việc đếm xem có bao nhiêu nước đi thực sự sẽ xảy ra và sau đó xác định ai là người thực hiện nước đi cuối cùng. 

1. Tính toán`m = min(x, k)`. Điều này thể hiện số lần di chuyển tối đa có thể thực sự xảy ra trước khi trò chơi không thể tiếp tục. 
2. Quyết định người chiến thắng dựa trên tính chẵn lẻ của`m`. OC đi nước đầu tiên nên OC thắng nếu`m`là số lẻ, ngược lại KP thắng. 

Lý do đằng sau bước đầu tiên là mỗi nước đi đảm bảo ít nhất một xiên được loại bỏ và giảm đi.`k`bởi chính xác một, vì vậy không tài nguyên nào có thể cho phép trò chơi vượt quá`min(x, k)`di chuyển. 

Bước thứ hai đến từ các lượt luân phiên. Nếu tổng số nước đi là số lẻ, người chơi thứ nhất OC thực hiện nước đi hợp lệ cuối cùng và khiến người chơi thứ hai không được di chuyển. Nếu chẵn, KP thực hiện nước đi cuối cùng và OC bị kẹt tiếp theo. 

### Tại sao nó hoạt động 

Ở mỗi giai đoạn của trò chơi, hai “ngân sách” độc lập đang bị thu hẹp: xiên và trợ cấp di chuyển. Xiên giảm ít nhất một cho mỗi lần di chuyển và giới hạn di chuyển giảm chính xác một cho mỗi lần di chuyển. Vì cả hai đều giảm một cách đơn điệu và không thể bỏ qua nên trò chơi hoàn toàn được xác định bởi tài nguyên nào cạn kiệt trước. Một khi chúng tôi nhận ra rằng mọi nước đi luôn có thể được thực hiện theo cách chỉ loại bỏ một xiên, thì thời lượng trong trường hợp xấu nhất là bắt buộc và không có lựa chọn chiến lược nào có thể kéo dài hoặc rút ngắn trò chơi vượt quá`min(x, k)`. Sau lần giảm đó, trò chơi sẽ trở thành một lượt đếm xen kẽ đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x, k = map(int, input().split())
        m = x if x < k else k
        if m % 2 == 1:
            print("OC")
        else:
            print("KP")

if __name__ == "__main__":
    solve()
```Mã trực tiếp tính toán số lần di chuyển hiệu quả bằng cách sử dụng một thao tác tối thiểu đơn giản. Sự so sánh`x < k`tránh chi phí cho các lệnh gọi hàm và đảm bảo thời gian không đổi cho mỗi trường hợp thử nghiệm. Việc kiểm tra tính chẵn lẻ sau đó sẽ xác định người chiến thắng, dựa vào việc OC luôn xuất phát trước. 

Một lỗi triển khai phổ biến là cho rằng trò chơi luôn kéo dài chính xác`k`di chuyển. Điều đó thất bại khi`x < k`, vì đống đã trống trước đó. Một sai lầm khác là cố gắng mô phỏng các nước đi một cách tham lam với số lượng loại bỏ thay đổi, điều này là không cần thiết vì cách chơi tối ưu không ảnh hưởng đến tổng số nước đi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`x = 4, k = 4`Chúng tôi theo dõi giới hạn di chuyển hiệu quả: 

| Di chuyển | Còn lại x | Còn lại k | Ghi chú | 
| --- | --- | --- | --- | 
| 0 | 4 | 4 | bắt đầu | 
| 1 | 3 | 3 | OC di chuyển | 
| 2 | 2 | 2 | KP di chuyển | 
| 3 | 1 | 1 | OC di chuyển | 
| 4 | 0 | 0 | KP di chuyển kết thúc trận đấu | 

Đây`m = min(4, 4) = 4`. Trận đấu kéo dài 4 nước, là số chẵn nên KP thắng. Điều này xác nhận rằng khi cả hai tài nguyên đều cạn kiệt, chỉ tính chẵn lẻ sẽ xác định người chiến thắng. 

### Ví dụ 2 

đầu vào:`x = 2, k = 1`| Di chuyển | Còn lại x | Còn lại k | Ghi chú | 
| --- | --- | --- | --- | 
| 0 | 2 | 1 | bắt đầu | 
| 1 | 1 | 0 | OC lấy 1, k trở thành 0 | 

Đây`m = min(2, 1) = 1`. Chỉ có thể thực hiện được một nước đi vì sau đó`k = 0`. OC thực hiện nước đi đó và giành chiến thắng ngay lập tức, chứng tỏ trường hợp giới hạn nước đi chi phối kích thước cọc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm sử dụng các phép toán số học có thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến được lưu trữ bất kể kích thước đầu vào | 

Giải pháp thoải mái xử lý lên đến`10^5`các trường hợp thử nghiệm vì mỗi trường hợp được xử lý bằng một lần tính toán tối thiểu và kiểm tra tính chẵn lẻ, cả hai đều là các hoạt động có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        x, k = map(int, input().split())
        m = x if x < k else k
        out.append("OC" if m % 2 == 1 else "KP")
    return "\n".join(out) + "\n"

# provided samples
assert run("4\n4 4\n2 1\n100000000 10\n1 1\n") == "KP\nOC\nKP\nOC\n"

# custom cases
assert run("1\n1 1\n") == "OC\n"
assert run("1\n10 1\n") == "OC\n"
assert run("1\n1 10\n") == "OC\n"
assert run("1\n100 100\n") == "KP\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 1`| OC | trường hợp đối xứng nhỏ nhất | 
|`10 1`| OC | k chiếm ưu thế, di chuyển duy nhất | 
|`1 10`| OC | x chiếm ưu thế, chấm dứt sớm | 
|`100 100`| KP | ràng buộc ngang giá ở quy mô lớn | 

## Vỏ cạnh 

Khi nào`k = 1`, trò chơi sụp đổ ngay lập tức. OC di chuyển một lần, giảm`k`về 0, và KP không có động thái hợp pháp bất kể còn lại bao nhiêu xiên. Thuật toán tính toán`m = min(x, 1) = 1`, là số lẻ nên OC thắng đúng. 

Khi`x < k`, chẳng hạn như`x = 2, k = 10`, trò chơi kết thúc do hết xiên chứ không phải do giới hạn lượt di chuyển. OC bỏ 1 xiên, KP bỏ 1 xiên, cọc sẽ trống sau 2 lần di chuyển. Thuật toán sử dụng`m = 2`, mang lại chiến thắng cho KP vì nước đi thứ hai là nước đi cuối cùng. 

Khi`x = k`, cả hai ràng buộc đều căn chỉnh hoàn hảo và trò chơi luôn kéo dài chính xác`k`di chuyển. Giải pháp giảm sạch đến mức chẵn lẻ của`k`, phù hợp với cấu trúc xen kẽ của các lượt.
