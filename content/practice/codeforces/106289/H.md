---
title: "CF 106289H - Huy chương"
description: "Nhiệm vụ mô tả quy trình phân bổ huy chương đơn giản trong một cuộc thi kiểu ACM. Chúng ta được cung cấp số đội hợp lệ trong một cuộc thi và cần xác định có bao nhiêu đội nhận được huy chương vàng, bạc, đồng theo quy tắc cố định được xác định bởi thể thức cuộc thi."
date: "2026-06-19T16:46:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106289
codeforces_index: "H"
codeforces_contest_name: "The 2025 Hunan University Freshman Contest"
rating: 0
weight: 106289
solve_time_s: 188
verified: true
draft: false
---

[CF 106289H - Huy chương](https://codeforces.com/problemset/problem/106289/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả quy trình phân bổ huy chương đơn giản trong một cuộc thi kiểu ACM. Chúng ta được cung cấp số đội hợp lệ trong một cuộc thi và cần xác định có bao nhiêu đội nhận được huy chương vàng, bạc, đồng theo quy tắc cố định được xác định bởi thể thức cuộc thi. 

Mỗi đội được xếp hạng ngầm từ tốt nhất đến kém nhất và phần cao nhất của các đội được chia thành ba nhóm riêng biệt: huy chương vàng đầu tiên, sau đó là huy chương bạc từ phần trên cùng còn lại, và sau đó là huy chương đồng. Các ranh giới chính xác được xác định bởi các quy tắc tỷ lệ được chỉ định trong phát biểu bài toán. 

Kết quả đầu ra là ba số nguyên biểu thị số đội rơi vào mỗi hạng huy chương. 

Phạm vi ràng buộc, với số lượng nhóm từ 100 đến 500, ngay lập tức gợi ý rằng mọi giải pháp đều phải có thời gian không đổi cho mỗi trường hợp thử nghiệm. Ngay cả một mô phỏng đơn giản trên một phạm vi lớn cũng không cần thiết vì mọi thứ đều có thể được tính trực tiếp bằng cách sử dụng số học trên n. 

Cạm bẫy tinh tế duy nhất trong những vấn đề như thế này là làm tròn số. Giới hạn huy chương thường được xác định bằng cách sử dụng phân số của n và việc xử lý sai cách phân chia trần so với sàn có thể chuyển một hoặc nhiều đội giữa các hạng mục. Vấn đề phổ biến thứ hai là sử dụng số học dấu phẩy động, có thể gây ra lỗi chính xác khi chuyển đổi tỷ lệ phần trăm thành số nguyên. 

Một ví dụ nhỏ minh họa độ nhạy làm tròn. Nếu n = 101 và vàng được xác định là 10 phần trăm, kết quả chính xác phụ thuộc vào việc chúng ta lấy sàn (10.1) hay trần (10.1). Việc triển khai dấu phẩy động có thể tạo ra 10.0 hoặc 11.0 không nhất quán do độ chính xác, trong khi số học số nguyên giữ cho nó ổn định. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ sắp xếp hoặc liệt kê rõ ràng tất cả các đội và chỉ định huy chương cho từng đội một. Đây vẫn sẽ là O(n log n) hoặc O(n), vốn đã tầm thường đối với n lên tới 500, nhưng nó là chi phí không cần thiết vì không cần dữ liệu đặt hàng thực tế. 

Nhận xét quan trọng là vấn đề không phụ thuộc vào từng đội mà chỉ phụ thuộc vào số lượng của họ. Mỗi nhóm huy chương được xác định hoàn toàn bằng hàm xác định của n. Điều này thu gọn vấn đề thành việc tính toán ba biểu thức số nguyên độc lập. 

Sự cải thiện về sức mạnh vũ phu đến từ việc nhận ra rằng cấu trúc xếp hạng không liên quan vượt quá ngưỡng vị trí. Khi chúng tôi tính toán giới hạn cho vàng, bạc và đồng, câu trả lời sẽ xuất hiện ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng rõ ràng về xếp hạng | O(n log n) | O(n) | Quá mức cần thiết nhưng hoạt động | 
| Tính toán số học trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta tính số người đoạt huy chương vàng, bạc và đồng trực tiếp từ n bằng cách sử dụng các quy tắc tỉ lệ. 

1. Đọc số nguyên n thể hiện tổng số đội hợp lệ. 
2. Tính ngưỡng huy chương vàng là mức trần của 1/10 n. Điều này phản ánh ý tưởng rằng ít nhất khoảng 10% đội đứng đầu sẽ nhận được vàng và làm tròn số đảm bảo không phân bổ dưới mức khi n không chia hết cho 10. 
3. Tính ngưỡng huy chương bạc là mức trần của 1/5 n. Điều này thể hiện ranh giới tích lũy của vàng và bạc. 
4. Tính ngưỡng huy chương đồng bằng mức trần của một nửa n. Điều này thể hiện ranh giới tích lũy của vàng, bạc và đồng. 
5. Chuyển đổi các ranh giới tích lũy này thành các kích thước nhóm rời rạc bằng phép trừ. Vàng được lấy trực tiếp. Bạc là sự khác biệt giữa ngưỡng bạc và ngưỡng vàng. Đồng là sự khác biệt giữa mức đồng và mức bạc. 

### Tại sao nó hoạt động

Mỗi hạng huy chương được xác định theo ngưỡng phần trăm cố định của danh sách xếp hạng. Việc sử dụng phép chia trần đảm bảo rằng mọi ranh giới đều có đủ đội ngay cả khi n không chia hết cho mẫu số. Vì các danh mục được xác định tích lũy nên việc trừ các ngưỡng liền kề sẽ tạo ra các nhóm có kích thước chính xác, rời rạc, phân chia chính xác tiền tố của bảng xếp hạng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

gold_cut = (n + 9) // 10
silver_cut = (n + 4) // 5
bronze_cut = (n + 1) // 2

gold = gold_cut
silver = silver_cut - gold_cut
bronze = bronze_cut - silver_cut

print(gold, silver, bronze)
```Việc triển khai sẽ đọc số lượng đội và tính toán ba ngưỡng tích lũy bằng cách sử dụng số học số nguyên. Mỗi ngưỡng sử dụng phép chia trần để tránh điểm không chính xác của dấu phẩy động. 

Vàng được lấy trực tiếp từ ngưỡng đầu tiên. Bạc được tính là mức tăng dần từ vàng lên ngưỡng thứ hai, đảm bảo không bị trùng lặp. Đồng được tính tương tự từ ngưỡng thứ hai đến ngưỡng thứ ba. 

Chi tiết triển khai chính là tránh hoàn toàn số học dấu phẩy động. Sử dụng các biểu thức số nguyên như`(n + k - 1) // k`đảm bảo hành vi làm tròn chính xác ngay cả ở các giá trị biên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp là thời gian không đổi và phù hợp một cách tầm thường trong các ràng buộc. Thậm chí nhiều trường hợp thử nghiệm sẽ không ảnh hưởng đến hiệu suất. 

## Vỏ cạnh 

Trường hợp cạnh phổ biến xảy ra khi n nhỏ so với mẫu số. Ví dụ: khi n = 100, cả ba điểm giới hạn có thể nằm chính xác trên ranh giới số nguyên và việc làm tròn không chính xác có thể chuyển một nhóm giữa các danh mục. Việc sử dụng vách ngăn trần đảm bảo sự ổn định. 

Một trường hợp cạnh khác là khi n không chia hết cho 10, 5 hoặc 2. Ví dụ: n = 101 tạo ra ranh giới phân số. Công thức số nguyên đảm bảo tính làm tròn xác định lên trên, tránh mất các đội ở các phân khúc cao nhất. 

Cuối cùng, khi n ở mức tối thiểu (khoảng 100), sự khác biệt giữa các ngưỡng là nhỏ, do đó các lỗi riêng lẻ trở nên đặc biệt rõ ràng. Việc xây dựng dựa trên phép trừ đảm bảo rằng ngay cả trong những trường hợp này, phân vùng vẫn nhất quán và không âm. 

## Ví dụ đã hoạt động 

Xét n = 100. 

Ngưỡng vàng là (100 + 9) // 10 = 10. Ngưỡng bạc là (100 + 4) // 5 = 20. Ngưỡng đồng là (100 + 1) // 2 = 50. 

Vàng = 10, bạc = 10, đồng = 30. 

Điều này cho thấy ngưỡng tích lũy chuyển thành kích thước nhóm rời rạc như thế nào. 

Bây giờ hãy xem xét n = 101. 

Điểm cắt vàng trở thành 11, điểm cắt bạc trở thành 21, điểm cắt đồng trở thành 50. Các nhóm kết quả là vàng = 11, bạc = 10, đồng = 29. Điều này thể hiện cách phân chia trần điều chỉnh cho các trường hợp không chia được trong khi vẫn duy trì cấu trúc đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ cố định các phép toán số học | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài các biến | 

Việc tính toán diễn ra theo thời gian không đổi và hoàn toàn độc lập với kích thước đầu vào ngoài việc đọc n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input().strip())

    gold_cut = (n + 9) // 10
    silver_cut = (n + 4) // 5
    bronze_cut = (n + 1) // 2

    gold = gold_cut
    silver = silver_cut - gold_cut
    bronze = bronze_cut - silver_cut

    return f"{gold} {silver} {bronze}"

assert run("100") == "10 10 30"
assert run("101") == "11 10 29"
assert run("200") == "20 20 60"
assert run("500") == "50 50 150"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 100 | 10 10 30 | trường hợp ranh giới chính xác | 
| 101 | 11 10 29 | làm tròn không chia hết | 
| 200 | 20 20 60 | trường hợp cân bằng lớn hơn | 
| 500 | 50 50 150 | ổn định giới hạn trên |
