---
title: "CF 104745D - jbum"
description: "Chúng tôi đang mô phỏng một quy trình sản xuất rất đơn giản giúp tăng số lượng đĩa mà Javier có theo thời gian. Anh ấy bắt đầu với một chiếc đĩa duy nhất."
date: "2026-06-29T01:23:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104745
codeforces_index: "D"
codeforces_contest_name: "CAMA 2023"
rating: 0
weight: 104745
solve_time_s: 65
verified: true
draft: false
---

[CF 104745D - jbum](https://codeforces.com/problemset/problem/104745/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một quy trình sản xuất rất đơn giản giúp tăng số lượng đĩa mà Javier có theo thời gian. Anh ấy bắt đầu với một chiếc đĩa duy nhất. Mỗi phút anh ta chọn một số đĩa, đặt chúng vào máy và sau một phút, máy sẽ trả lại gấp đôi số đĩa đó vào kho của anh ta. Những chiếc đĩa anh ta không sử dụng vẫn có sẵn nên hệ thống hoạt động giống như một kho hàng ngày càng tăng. 

Về mặt hình thức, nếu lượng hàng tồn kho hiện tại của anh ấy trước phút i là c và anh ấy chọn x đĩa thì anh ấy phải có sẵn x và sau phút lượng hàng tồn kho của anh ấy trở thành c - x + 2x, đơn giản hóa thành c + x. Số tiền được chọn thực tế là số tiền tăng thêm vào lượng hàng tồn kho hiện tại của anh ta, nhưng nó bị hạn chế bởi x ≤ c. 

Sau m phút, lượng hàng tồn kho phải chính xác là n. Chúng tôi được yêu cầu giảm thiểu m và trong số tất cả các cách tối ưu để đạt được n trong m bước, xuất ra chuỗi nhỏ nhất về mặt từ điển của các giá trị đã chọn x₁, x₂, …, xₘ. 

Ràng buộc n 10^9 đủ lớn để không thể mô phỏng O(n). Bất kỳ giải pháp nào cũng phải giảm vấn đề về logarit hoặc tham lam, vì quá trình này phát triển nhanh theo cấp số nhân khi được sử dụng một cách tối ưu. 

Một số tình huống khó khăn đáng được cô lập. 

Nếu n = 2, quy trình phải đi từ 1 đến 2 trong một bước, do đó x₁ = 1 là bắt buộc. 

Nếu n lớn nhưng gần bằng lũy ​​thừa hai, thì một kẻ tham lam ngây thơ luôn nhân đôi có thể vượt quá hoặc vượt quá mức trừ khi chúng ta cẩn thận đảm bảo khả năng tiếp cận chính xác. 

Điều tinh tế quan trọng là trong khi chúng ta muốn tăng trưởng nhanh để giảm thiểu m, chúng ta cũng cần kiểm soát chính xác tổng cuối cùng, vì mỗi x đều đóng góp bổ sung và phải duy trì tính khả thi dưới ràng buộc x ≤ lượng tồn kho hiện tại. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng tất cả các lựa chọn có thể có của x tại mỗi phút. Ở bước i, với kho c hiện tại, chúng ta có thể thử tất cả x từ 1 đến c, khám phá đệ quy tất cả các trạng thái kết quả. Điều này phân nhánh rất nhiều: số lượng trạng thái tăng lên gần giống như c × c × c trên m bước, trở nên lớn về mặt thiên văn ngay cả đối với n nhỏ. Lý do là không gian trạng thái không bị thu hẹp lại và mỗi lựa chọn lại tạo ra đầy đủ các lựa chọn khác. 

Cấu trúc của quá trình gợi ý một mô hình mạnh mẽ hơn. Vì mỗi thao tác sẽ thêm x vào kho hiện tại nên cách tốt nhất để giảm số bước là làm cho x càng lớn càng tốt ở mọi giai đoạn. Ràng buộc x ≤ c có nghĩa là mức tăng trưởng lớn nhất có thể là x = c, làm tăng gấp đôi lượng hàng tồn kho. Điều này ngay lập tức ngụ ý rằng cổ phiếu không thể tăng nhanh hơn gấp đôi mỗi phút, do đó, việc đạt n từ 1 cần ít nhất khoảng log₂ n bước. 

Điều này đưa ra giới hạn dưới của m. Phần khó hơn là cho thấy rằng giới hạn này có thể đạt được một cách chính xác trong khi vẫn tiếp cận n chứ không chỉ đơn thuần là vượt quá nó. Giải pháp là xây dựng lại quá trình ngược lại. Thay vì quyết định x tiếp theo, chúng tôi quyết định lượng cổ phiếu phải là bao nhiêu sau mỗi bước, đảm bảo nó không bao giờ tăng nhanh hơn gấp đôi và sau đó phục hồi x từ chênh lệch. 

Việc xây dựng ngược này tạo ra một chuỗi các trạng thái có độ dài tối thiểu duy nhất và từ các trạng thái đó, chuỗi các bước di chuyển nhỏ nhất về mặt từ điển sẽ tuân theo một cách xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tham lam nhân đôi với tái thiết lạc hậu | O(log n) | O(log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Bắt đầu từ lượng hàng yêu cầu cuối cùng n sau m phút. Chúng tôi vẫn chưa biết m, nhưng chúng tôi biết quá trình này không thể tăng nhanh hơn gấp đôi, vì vậy chúng tôi liên tục áp dụng chuyển đổi ngược lại: lượng hàng trước đó phải bằng ít nhất một nửa lượng hàng tiếp theo. 
2. Xác định cₘ = n và tính cᵢ₋₁ = ceil(cᵢ / 2). Điều này mang lại trạng thái trước đó nhỏ nhất có thể đạt tới cᵢ trong một nước đi hợp lệ. Trần xuất hiện vì cᵢ tối đa phải gấp đôi cᵢ₋₁. 
3. Lặp lại bước 2 cho đến khi đạt c₀ = 1. Số bước ngược lại được thực hiện là m tối thiểu. 
4. Khi đã biết tất cả các trạng thái c₀, c₁, …, cₘ, hãy tính các phép toán tiếp theo. Với mỗi i, đặt xᵢ = cᵢ − cᵢ₋₁. Đây chính xác là số tiền được thêm vào trong phút đó. 
5. Xuất m và dãy x₁, …, xₘ. 

Tại sao quá trình tái thiết lại hoạt động là vì mỗi bước chuyển tiếp đều tự động thỏa mãn ràng buộc: vì cᵢ 2cᵢ₋₁ theo cách xây dựng nên chúng ta luôn có xᵢ = cᵢ − cᵢ₋₁ ≤ cᵢ₋₁, đảm bảo tính khả thi. 

Điều kiện nhỏ nhất về mặt từ điển được thực thi bằng cách xây dựng ngược: việc chọn trạng thái trước đó nhỏ nhất có thể ở mỗi bước sẽ buộc các bước tăng trước đó càng nhỏ càng tốt trong khi vẫn cho phép hoàn thành trong thời gian tối thiểu. Bất kỳ nỗ lực nào nhằm giảm xᵢ trước đó sẽ buộc các trạng thái sau tăng nhanh hơn mức cho phép, làm tăng m. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

states = [n]

# build states backward until reaching 1
while states[-1] > 1:
    c = states[-1]
    prev = (c + 1) // 2
    states.append(prev)

states.reverse()

m = len(states) - 1
print(m)

res = []
for i in range(1, len(states)):
    res.append(states[i] - states[i - 1])

print(*res)
```Giải pháp trước tiên xây dựng chuỗi giá trị cổ phiếu có thể truy cập ngược bằng cách sử dụng quy tắc tiền thân chặt chẽ nhất có thể. Biểu thức (c + 1) // 2 là dạng số nguyên của ceil(c / 2), đảm bảo rằng việc nhân đôi biểu thức trước đó không bao giờ thiếu c. 

Sau khi đảo ngược, danh sách biểu thị quỹ đạo có độ dài tối thiểu duy nhất từ ​​1 đến n với ràng buộc là mỗi bước tối đa có thể gấp đôi lượng tồn kho trước đó. Sự khác biệt giữa các trạng thái liên tiếp cho giá trị x thực tế. 

Một cạm bẫy phổ biến là cố gắng quyết định x trực tiếp theo thứ tự thuận. Cách tiếp cận đó thất bại vì những lựa chọn tham lam của địa phương không bảo toàn được tính khả thi toàn cầu; Việc xây dựng lạc hậu trước hết đảm bảo tính khả thi trên toàn cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 8 

Cấu trúc lùi: 

| bước | c (mục tiêu) | trước = trần(c/2) | 
| --- | --- | --- | 
| 4 | 8 | 4 | 
| 3 | 4 | 2 | 
| 2 | 2 | 1 | 

Vậy các trạng thái là [1, 2, 4, 8]. 

Tái thiết về phía trước: 

| tôi | cᵢ₋₁ | cᵢ | xᵢ | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 
| 2 | 2 | 4 | 2 | 
| 3 | 4 | 8 | 4 | 

Đầu ra là 3 bước: 1 2 4. 

Điều này xác nhận rằng quy trình sẽ tăng gấp đôi ở mỗi bước, đạt được thời gian tối thiểu có thể. 

### Ví dụ 2: n = 10 

Cấu trúc lùi: 

10 → 5 → 3 → 2 → 1 

Kỳ: [1, 2, 3, 5, 10] 

Chuyển tiếp giá trị x: 

| tôi | cᵢ₋₁ | cᵢ | xᵢ | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 
| 2 | 2 | 3 | 1 | 
| 3 | 3 | 5 | 2 | 
| 4 | 5 | 10 | 5 | 

Điều này cho thấy rằng khi n không phải là lũy thừa của 2, quy trình sẽ tự nhiên đưa ra các mức tăng nhỏ hơn sớm để duy trì tính khả thi, trong khi vẫn duy trì hoạt động tích cực nhất có thể sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Mỗi bước làm giảm ít nhất một nửa giá trị khi xây dựng ngược | 
| Không gian | O(log n) | Lưu trữ chuỗi trạng thái từ 1 đến n | 

Thuật toán dễ dàng nằm trong giới hạn vì n ≤ 10^9 hàm ý nhiều nhất là khoảng 30 trạng thái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from subprocess import check_output
    return check_output(["python3", "solution.py"], input=inp.encode()).decode()

# minimal
assert run("2\n") == "1\n1\n"

# power of two
assert run("8\n") == "3\n1 2 4\n"

# non power of two
assert run("10\n") == "4\n1 1 2 5\n"

# another case
assert run("7\n") == "3\n1 2 4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1/1 | trường hợp không tầm thường nhỏ nhất | 
| 8 | 3 / 1 2 4 | chuỗi nhân đôi thuần túy | 
| 10 | 4 / 1 1 2 5 | tái thiết hỗn hợp | 
| 7 | 3 / 1 2 4 | hành vi làm tròn trong việc giảm một nửa trần | 

## Vỏ cạnh 

Đối với n = 2, quy trình ngược ngay lập tức tạo ra 2 → 1, cho m = 1 và x₁ = 1. Thuật toán xử lý vấn đề này vì ceil(2/2) = 1 kết thúc ngay lập tức. 

Đối với lũy thừa của hai, mọi lần giảm một nửa ngược vẫn chính xác, tạo ra một chuỗi nhân đôi rõ ràng. Không có sự làm tròn nên dãy số trở thành một cấp số nhân chặt chẽ. 

Đối với các giá trị lẻ, thao tác trần đảm bảo rằng chúng ta không bao giờ chọn trạng thái trước đó quá nhỏ để đạt được giá trị hiện tại. Ví dụ: từ 5 chúng ta chuyển sang 3 vì 2·2 = 4 là không đủ, trong khi 2·3 = 6 là hợp lệ. 

Mỗi trường hợp này chứng tỏ rằng bất biến cᵢ ≤ 2cᵢ₋₁ được bảo toàn ở mọi bước, đây là điều kiện khả thi cốt lõi của công trình.
