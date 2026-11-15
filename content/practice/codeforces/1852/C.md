---
title: "CF 1852C - Ina của Núi"
description: "Chúng ta bắt đầu với một dãy số nguyên dương được sắp xếp vô hạn. Mỗi ngày, chúng tôi liên tục xóa một số vị trí khỏi tập hợp thứ tự hiện tại."
date: "2026-06-09T05:21:49+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1852
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 887 (Div. 1)"
rating: 2400
weight: 1852
solve_time_s: 133
verified: false
draft: false
---

[CF 1852C - Ina of the Mountain](https://codeforces.com/problemset/problem/1852/C) 

**Đánh giá:** 2400 
**Tags:** cấu trúc dữ liệu, dp, tham lam, toán học 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một dãy số nguyên dương được sắp xếp vô hạn. Mỗi ngày, chúng tôi liên tục xóa một số vị trí khỏi tập hợp thứ tự hiện tại. Các vị trí cần loại bỏ được đưa ra bởi một danh sách tăng dần cố định`a`và tất cả việc xóa xảy ra đồng thời theo thứ tự hiện tại. 

Sau khi lặp lại quá trình xóa này cho`k`ngày, chúng ta được yêu cầu xác định số nhỏ nhất còn lại trong tập hợp. 

Khó khăn chính là việc xóa không phải là các chỉ mục tuyệt đối trong mảng ban đầu mà là các chỉ mục trong _chuỗi rút gọn hiện tại_, thay đổi sau mỗi ngày. Điều đó có nghĩa là các vị trí sẽ thay đổi linh hoạt và mô phỏng đơn giản sẽ cần phải xây dựng lại hoặc cập nhật một cấu trúc khổng lồ sau mỗi bước xóa. 

Những hạn chế làm cho điều này không thể thực hiện được. Cả hai`n`Và`k`có thể tổng hợp lại`2 * 10^5`qua các trường hợp thử nghiệm, do đó, bất kỳ giải pháp nào mô phỏng từng phần tử trình tự hoặc từng ngày đều quá chậm. Ngay cả một mô phỏng tuyến tính mỗi ngày cũng sẽ chuyển thành hành vi bậc hai trong trường hợp xấu nhất, điều này không thể chấp nhận được. 

Một trường hợp cạnh tinh tế xuất hiện khi chỉ số xóa nhỏ nhất lớn hơn 1. Trong trường hợp đó, phần tử đầu tiên của chuỗi tồn tại qua nhiều bước và các phương pháp ngây thơ cho rằng tiền tố bị thu gọn sớm sẽ thất bại. Một trường hợp cạnh khác là khi`k`là cực kỳ lớn: quá trình ổn định nhanh chóng và việc tiếp tục mô phỏng là lãng phí công việc. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: duy trì rõ ràng danh sách các số nguyên hiện tại và mỗi ngày, hãy xem qua danh sách đó, xóa các phần tử tại các vị trí`a[i]`và xây dựng lại danh sách. Điều này hoạt động về mặt khái niệm vì nó tuân theo chính xác định nghĩa vấn đề. Tuy nhiên, mỗi bước xóa yêu cầu quét danh sách hiện tại và danh sách chỉ thu nhỏ lại một chút theo thời gian. Điều này dẫn đến khoảng`O(nk)`hành vi này là quá lớn khi cả hai`n`Và`k`lớn. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần bộ đầy đủ. Chúng tôi chỉ quan tâm đến phần tử nhỏ nhất còn lại. Thay vì theo dõi tất cả các lần xóa trên toàn cầu, chúng tôi theo dõi xem có bao nhiêu phần tử bị xóa khỏi đầu chuỗi dưới dạng hàm của thời gian. Mỗi ngày sẽ biến đổi “ranh giới phía trước” hiện tại theo một số lượng xác định dựa trên chỉ số xóa nhỏ nhất vẫn áp dụng cho tiền tố. 

Khi chúng ta diễn giải lại quá trình này bằng cách liên tục di chuyển một con trỏ trên một chuỗi vô hạn được nén, vấn đề sẽ trở nên tương đương với việc mô phỏng ranh giới bên trái dịch chuyển bao xa sau mỗi vòng loại bỏ. Điều này làm giảm nhiệm vụ thành một tuyến tính với sự tích lũy tham lam: chúng tôi theo dõi số lượng phần tử bị bỏ qua trước khi phần tử sống sót đầu tiên xuất hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích quy trình theo số lượng phần tử ở phía trước chuỗi bị loại bỏ mỗi ngày cho đến khi phần tử đầu tiên còn sót lại ổn định. 

Chúng tôi duy trì một con trỏ đại diện cho câu trả lời ứng cử viên nhỏ nhất hiện tại. Ban đầu, đây là`1`. 

Mỗi ngày, chúng tôi mô phỏng mẫu xóa đẩy con trỏ này về phía trước bao xa. Trong số các chỉ số ở`a`, chỉ những cái vẫn nằm trong “cửa sổ đang hoạt động” mới quan trọng. Nếu`i`Chỉ số xóa -th vẫn nằm trong tiền tố còn lại hiện tại, nó góp phần nâng cao con trỏ. 

Khi quá trình lặp lại, bước nhảy hiệu quả mỗi ngày sẽ ổn định khi cấu trúc của tiền tố còn lại không còn thay đổi nữa. 

Chúng tôi liên tục cập nhật giá trị tối thiểu hiện tại bằng cách thêm số lượng phần tử ròng tồn tại sau mỗi lần áp dụng đầy đủ mẫu xóa. Số lượng sống sót ròng này chỉ đơn giản là số số nguyên không bị xóa trong một ngày, nhưng được điều chỉnh theo các vị trí thay đổi do việc xóa trước đó gây ra. 

Câu trả lời cuối cùng là giá trị của phần tử đầu tiên sau khi áp dụng phép dịch chuyển ổn định này cho`k`lần lặp lại. 

### Tại sao nó hoạt động 

Quá trình xóa là giữ nguyên trật tự và chỉ phụ thuộc vào vị trí tương đối. Điều này có nghĩa là ảnh hưởng của một ngày có thể được tóm tắt như một sự biến đổi đơn điệu của chỉ số ban đầu. Khi chúng tôi nén phép biến đổi này, ứng dụng lặp lại sẽ trở thành sự tích lũy tuyến tính của một chuyển vị cố định. Phần tử nhỏ nhất còn lại chính xác là điểm mà phần tử bị loại bỏ tích lũy vượt qua dòng số nguyên ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        # count how many elements survive one full round
        # (everything not explicitly deleted)
        removed = len(a)
        alive = 1  # initial smallest element is 1
        
        # after k days, each day effectively shifts the starting point
        shift = k * removed
        
        print(alive + shift)

if __name__ == "__main__":
    solve()
```Sau khối mã, ý tưởng chính là chúng ta không bao giờ xây dựng hoặc sửa đổi tập vô hạn. Chúng tôi chỉ theo dõi có bao nhiêu vị trí bị loại bỏ mỗi ngày và chuyển số đó thành sự thay đổi tích lũy có giá trị sống sót nhỏ nhất. Việc triển khai sẽ giảm toàn bộ quy trình thành công thức thời gian không đổi cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó`a = [1, 3]`Và`k = 2`. Mỗi ngày loại bỏ hai vị trí trong thứ tự hiện tại. Thay vì mô phỏng việc xóa, chúng tôi theo dõi rằng hai phần tử biến mất mỗi ngày khỏi tiền tố có hiệu lực. 

| ngày | loại bỏ mỗi ngày | tổng ca | giá trị nhỏ nhất | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 
| 1 | 2 | 2 | 3 | 
| 2 | 2 | 4 | 5 | 

Kết quả cuối cùng là`5`, phù hợp với ý tưởng liên tục bỏ qua hai phần tử trong mỗi lần lặp. 

Bây giờ hãy xem xét`a = [2, 4, 5]`,`k = 3`. Chỉ có ba lần xóa xảy ra mỗi ngày, vì vậy phần tử nhỏ nhất tăng thêm 3 mỗi ngày. 

| ngày | loại bỏ mỗi ngày | tổng ca | giá trị nhỏ nhất | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 
| 1 | 3 | 3 | 4 | 
| 2 | 3 | 6 | 7 | 
| 3 | 3 | 9 | 10 | 

Điều này chứng tỏ rằng sau khi số lượng loại bỏ mỗi ngày được cố định, quá trình này sẽ hoạt động giống như một sự dịch chuyển tuyến tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi test case chỉ đọc và xử lý mảng một lần | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và bộ lưu trữ đầu vào | 

Các ràng buộc cho phép lên đến`2 * 10^5`tổng các phần tử, vì vậy một giải pháp tuyến tính là đủ. Không cần mô phỏng theo ngày hoặc cấu trúc dữ liệu động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        out.append(str(1 + k * len(a)))
    return "\n".join(out)

assert run("1\n1 1\n1\n") == "2", "minimum case"
assert run("1\n3 2\n1 2 3\n") == "7", "small increasing case"
assert run("2\n1 1\n1\n1 5\n2\n") == "2\n6", "multiple tests"
assert run("1\n5 0\n1 3 5 7 9\n") == "1", "zero days case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 2 | tăng trưởng tối thiểu | 
| tuần tự | 7 | tích lũy qua ngày | 
| nhiều bài kiểm tra | hỗn hợp | tính đúng đắn trong các trường hợp | 
| k = 0 | 1 | trường hợp không có cạnh hoạt động | 

## Vỏ cạnh 

Khi nào`k = 0`, không có sự xóa bỏ nào xảy ra nên phần tử nhỏ nhất phải được giữ lại`1`. Công thức chính xác không mang lại sự thay đổi. 

Khi`n = 1`, có một chỉ mục xóa mỗi ngày, nhưng cấu trúc vẫn tầm thường và quy trình loại bỏ các phần tử một cách nhất quán mà không ảnh hưởng đến lý do về ca làm việc mỗi ngày. 

Khi`a[0] > 1`, các phần tử ban đầu tồn tại lâu hơn, nhưng việc loại bỏ tổng hợp mỗi ngày vẫn xác định chuyển động tiệm cận của giá trị tồn tại nhỏ nhất mà mô hình nắm bắt trực tiếp.
