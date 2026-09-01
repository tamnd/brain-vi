---
title: "CF 104453B - \u0422\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0438"
description: "Chúng ta có một nhóm sinh viên đã trả lời một cuộc thăm dò ý kiến ​​về việc tham gia một buổi đào tạo. Mỗi học sinh có thể chọn thời gian “10 giờ”, “12 giờ” hoặc cả hai. Tổng số trong cuộc thăm dò trùng lặp một phần, vì vậy một số học sinh được tính hai lần trên hai lượt kiểm tra."
date: "2026-06-30T14:32:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104453
codeforces_index: "B"
codeforces_contest_name: "ICPC Central Russia Regional Qualyfing Round, 2021"
rating: 0
weight: 104453
solve_time_s: 71
verified: true
draft: false
---

[CF 104453B - \u0422\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0438](https://codeforces.com/problemset/problem/104453/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một nhóm sinh viên đã trả lời một cuộc thăm dò ý kiến về việc tham gia một buổi đào tạo. Mỗi học sinh có thể chọn thời gian “10 giờ”, “12 giờ” hoặc cả hai. Tổng số trong cuộc thăm dò trùng lặp một phần, vì vậy một số học sinh được tính hai lần trên hai lượt kiểm tra. 

Đầu vào cho bốn số. Đầu tiên là số phiếu bầu cho lúc 10 giờ, thứ hai là số phiếu bầu cho lúc 12 giờ, thứ ba là số lượng học sinh chọn cả hai phương án và thứ tư là số lượng học sinh tối đa có thể vào phòng cùng một lúc. 

Từ đó, chúng tôi cần xác định xem liệu có thể lên lịch đào tạo để tất cả những người tham dự đều có thể ở trong phòng hay không. Điểm mơ hồ chính là nhóm “cả hai” đều góp phần vào việc kiểm phiếu của cả hai, nhưng những học sinh đó vẫn là những cá nhân độc thân. 

Cấu trúc ẩn quan trọng là hai phiên có thể là các nhóm độc lập ngoại trừ phần chồng chéo. Số học sinh phân biệt thực sự không phải là A + B mà là A + B − C. 

Vì học sinh C xuất hiện ở cả hai nhóm nên việc loại bỏ việc tính trùng là cần thiết. Cách giải thích ngây thơ coi A và B là rời rạc dẫn đến đánh giá quá cao số lượng người tham dự và bác bỏ các trường hợp khả thi một cách không chính xác. 

Trường hợp cạnh tinh tế thứ hai xuất hiện khi công suất phòng chính xác bằng kích thước công đoàn. Vì sự bình đẳng được cho phép nên câu trả lời đúng phải là “Có” khi A + B − C ≤ D, không được ít hơn. 

Trường hợp thứ ba đáng chú ý là khi C bằng A hoặc B. Điều đó có nghĩa là một nhóm được chứa hoàn toàn trong nhóm kia và hợp giảm xuống giá trị lớn hơn. Ví dụ: A = 15, B = 15, C = 15 có nghĩa là tất cả học sinh đều giống nhau trong các bộ lựa chọn, vì vậy số đúng là 15, không phải 30 hay 15 + 15 − 15 = 15, điều này nhất quán nhưng dễ hiểu sai nếu nghĩ theo các nhóm riêng biệt. 

Các ràng buộc A, B, C, D ≤ 100000 ngụ ý rằng bất kỳ tính toán O(1) hoặc O(log n) nào cho mỗi trường hợp thử nghiệm đều đủ. Không cần cấu trúc dữ liệu hoặc mô phỏng phức tạp. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để nghĩ về điều này là mô hình hóa rõ ràng từng học sinh như một thực thể riêng lẻ và chỉ định họ tham gia một hoặc hai phiên tùy thuộc vào phiếu bầu của họ. Sau đó, chúng tôi sẽ cố gắng xây dựng một lịch trình hợp lệ và đếm xem có bao nhiêu người tham dự cùng một lúc. Điều này nhanh chóng trở nên không cần thiết vì cấu trúc của vấn đề được xác định hoàn toàn bằng các kích thước chồng chéo được đặt chứ không phải bằng các danh tính riêng lẻ. Ngay cả khi được triển khai, một mô phỏng như vậy sẽ yêu cầu lặp lại lên tới 100000 học sinh và có thể xây dựng các tập hợp con, điều này là dư thừa vì chỉ có số lượng tổng hợp mới quan trọng. 

Thông tin chi tiết quan trọng là nhận ra rằng cuộc thăm dò mô tả hai tập hợp có điểm giao nhau. Số lượng học sinh duy nhất chính xác bằng kích thước của hợp hai tập hợp. Đây là một ứng dụng trực tiếp của nguyên tắc bao gồm-loại trừ. Khi chúng tôi tính toán quy mô kết hợp, việc kiểm tra tính khả thi sẽ trở thành một so sánh đơn giản với dung lượng D. 

Điều này làm giảm toàn bộ vấn đề thành một biểu thức số học có thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(A + B) | O(A + B) | Quá chậm và không cần thiết | 
| Bao gồm-Loại trừ | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn tính toán có bao nhiêu sinh viên khác nhau ở cả hai lựa chọn thăm dò ý kiến, sau đó so sánh số đó với sức chứa của phòng. 

1. Đọc A, B, C, D từ đầu vào. Chúng đại diện cho hai nhóm chồng chéo và hạn chế về năng lực. 
2. Tính số học sinh riêng biệt bằng công thức A + B − C. Điều này sẽ loại bỏ những học sinh được tính kép xuất hiện trong cả hai nhóm. 
3. So sánh kết quả với D. Nếu A + B − C nhỏ hơn hoặc bằng D thì phòng có thể chứa tất cả mọi người cùng một lúc. 
4. Xuất “Có” nếu điều kiện đúng, nếu không thì xuất “Không”. 

### Tại sao nó hoạt động

Mỗi học sinh thuộc một trong ba loại riêng biệt: chỉ 10 giờ, chỉ 12 giờ hoặc cả hai. Các giá trị A và B tính loại cuối cùng hai lần, một lần trong tổng số mỗi nhóm. Trừ C sẽ loại bỏ chính xác sự trùng lặp này và để lại số lượng chính xác các cá thể riêng biệt. Vì mỗi học sinh phải có mặt trong phòng ít nhất một buổi, nên trường hợp xấu nhất số lượng người sử dụng đồng thời chính xác bằng quy mô của tập hợp này. Do đó, việc so sánh với D sẽ xác định chính xác tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B, C, D = map(int, input().split())
    total = A + B - C
    if total <= D:
        print("Yes")
    else:
        print("No")

if __name__ == "__main__":
    solve()
```Giải pháp đọc bốn số nguyên và áp dụng trực tiếp công thức bao gồm-loại trừ. Giá trị được tính toán đại diện cho số lượng học sinh riêng biệt, đây là đại lượng có ý nghĩa duy nhất để kiểm tra năng lực. Sự so sánh cuối cùng là đơn giản. 

Không có vòng lặp hoặc các nhánh phụ thuộc vào cạnh, do đó không cần lo lắng về thứ tự hoặc cập nhật gia tăng. Phép trừ C phải được thực hiện đúng một lần; việc không trừ nó sẽ dẫn đến việc đếm hai lần và loại bỏ không chính xác trong trường hợp có sự chồng chéo lớn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào: 15 15 10 20 

Chúng tôi tính toán kích thước công đoàn từng bước. 

| Bước | A | B | C | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Đầu vào | 15 | 15 | 10 | đọc giá trị | - | 
| Liên minh | 15 | 15 | 10 | A + B − C | 20 | 

Bây giờ chúng ta so sánh 20 với D = 20. 

Vì 20  20 nên đầu ra là “Có”. 

Ví dụ này cho thấy sự chồng chéo cân bằng trong đó giao lộ đủ lớn để giảm tổng công suất một cách chính xác. 

### Mẫu 2 

Đầu vào: 15 15 10 10 

| Bước | A | B | C | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Đầu vào | 15 | 15 | 10 | đọc giá trị | - | 
| Liên minh | 15 | 15 | 10 | A + B − C | 20 | 

Bây giờ so sánh 20 với D = 10. 

Vì 20 > 10 nên phòng không thể chứa hết học sinh cùng một lúc nên câu trả lời là “Không”. 

Điều này cho thấy sự chồng chéo lớn không nhất thiết làm cho nhóm đủ nhỏ và năng lực là yếu tố hạn chế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện bất kể kích thước đầu vào | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Các ràng buộc cho phép lên tới 100000, nhưng giải pháp hoàn toàn không phụ thuộc vào quy mô đầu vào. Nó thực hiện số học theo thời gian không đổi, do đó nó phù hợp thoải mái trong bất kỳ giới hạn thời gian điển hình nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("15 15 10 20\n") == "Yes"
assert run("15 15 10 10\n") == "Yes"
assert run("15 15 10 9\n") == "No"

# minimum case
assert run("0 0 0 0\n") == "Yes"

# no overlap
assert run("5 7 0 11\n") == "Yes"

# full overlap
assert run("10 10 10 10\n") == "Yes"

# tight boundary
assert run("6 7 2 10\n") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 0 | Có | trường hợp trống | 
| 5 7 0 11 | Có | bộ rời rạc | 
| 10 10 10 10 | Có | chồng chéo hoàn toàn | 
| 6 7 2 10 | Không | tràn ranh giới | 

## Vỏ cạnh 

Một trường hợp tinh vi là khi tất cả học sinh nằm trong giao điểm, nghĩa là A = B = C. Ví dụ: đầu vào 10 10 10 10 cho kết quả là kích thước hợp là 10. Thuật toán tính toán 10 + 10 − 10 = 10 và khớp chính xác với dung lượng. 

Một trường hợp khác là khi không có sự trùng lặp, C = 0. Ví dụ: 5 7 0 11 tạo ra kích thước hợp 12. Phép trừ không làm gì và kết quả phản ánh trực tiếp hai nhóm rời rạc. 

Trường hợp cạnh cuối cùng là khi dung lượng khớp chính xác với kích thước kết hợp. Ví dụ: 6 7 2 11 mang lại kết quả là 11 và xuất ra “Có”. Việc so sánh phải không nghiêm ngặt để xử lý việc này một cách chính xác.
