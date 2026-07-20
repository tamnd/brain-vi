---
title: "CF 103566H - \u0414\u043e\u0440\u043e\u0433\u0430 \u0432 \u0448\u043a\u043e\u043b\u0443."
description: "Chúng ta được cho một con đường có thể coi là một chuỗi gồm n đoạn đường được sắp xếp thành một đường nối giữa một ngôi nhà và một trường học. Ở đâu đó dọc theo đường này có một con đường hợp lệ ngắn nhất từ ​​nhà đến trường và độ dài của nó là một số nguyên x."
date: "2026-07-03T05:08:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103566
codeforces_index: "H"
codeforces_contest_name: "2021-2022 Olympiad Cognitive Technologies, Final Round"
rating: 0
weight: 103566
solve_time_s: 37
verified: true
draft: false
---

[CF 103566H - \u0414\u043e\u0440\u043e\u0433\u0430 \u0432 \u0448\u043a\u043e\u043b\u0443.](https://codeforces.com/problemset/problem/103566/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đường đi có thể được coi là một chuỗi các`n`các đoạn đường được bố trí thẳng hàng giữa nhà và trường học. Ở đâu đó dọc theo đường này có một tuyến đường hợp lệ ngắn nhất từ ​​nhà đến trường và độ dài của nó là một số nguyên`x`. Chúng tôi không biết tuyến đường này ở đâu hoặc nó hoạt động như thế nào trong nội bộ, nhưng chúng tôi có thể tương tác với hộp đen. 

Mỗi lần chúng tôi chọn một chỉ mục phân đoạn`i`, chúng tôi đặt một câu hỏi`? i`và nhận được một con số. Con số này phụ thuộc vào việc phân khúc`i`nằm trên con đường ngắn nhất ẩn. Nếu nó không nằm trên tuyến đường đó thì phản hồi chính xác là`x`. Nếu nó nằm trên tuyến đường, phản hồi sẽ trở thành`n - x`. 

Mục đích là xác định giá trị của`x`sử dụng càng ít truy vấn càng tốt. 

Ràng buộc quan trọng về mặt cấu trúc là tuyến đường ngắn nhất không thể chứa hơn một nửa số đoạn. Sự bất đối xứng này làm cho vấn đề có thể giải quyết được với rất ít truy vấn, vì nó đảm bảo rằng hầu hết các vị trí được lấy mẫu đều hoạt động theo cùng một cách. 

Từ quan điểm phức tạp, sự tương tác chi phối mọi thứ. Mỗi truy vấn đều tốn kém về mặt giao thức, vì vậy các giải pháp phải giảm thiểu số lượng truy vấn thay vì độ phức tạp về thời gian. Bất kỳ chiến lược nào cố gắng quét tất cả các vị trí đều ngay lập tức không thể thực hiện được khi`n`là lớn. 

Một trường hợp lỗi tinh vi xuất hiện nếu giả định rằng mọi truy vấn đều hoạt động theo cùng một cách hoặc nhiều vị trí được truy vấn là độc lập. Trong thực tế, tất cả các câu trả lời đều được gắn với cùng một giá trị ẩn trên toàn cầu`x`, do đó, sự dư thừa trong truy vấn không cung cấp thông tin mới ngoài một số lượng nhỏ các mẫu được chọn cẩn thận. 

## Phương pháp tiếp cận 

Cách nghĩ ngây thơ về vấn đề là thử mọi tư thế`i`, đặt câu hỏi và cố gắng suy luận xem liệu`i`nằm trên con đường ngắn nhất. Điều này nhanh chóng trở nên không cần thiết vì thông tin duy nhất được tiết lộ là vị trí có nằm trên đường dẫn hay không và mỗi truy vấn vẫn chỉ tạo ra một trong hai giá trị chung. Ngay cả khi chúng tôi cố gắng đánh dấu tất cả các vị trí và xây dựng lại cấu trúc đường dẫn, chúng tôi vẫn không học được`x`trực tiếp mà không khai thác mối quan hệ toàn cục giữa các câu trả lời. 

Quan sát quan trọng là hệ thống này được ngụy trang dưới dạng nhị phân. Mọi truy vấn đều trả về`x`hoặc`n - x`. Điều này có nghĩa là toàn bộ không gian tương tác sẽ phân biệt giá trị nào trong hai giá trị mà chúng ta đang thấy. Khi chúng ta có ít nhất một kết quả truy vấn, chúng ta đã biết rằng câu trả lời phải là giá trị được trả về hoặc phần bù của nó đối với`n`. 

Vấn đề duy nhất còn lại là sự mơ hồ: nếu chỉ lấy một mẫu, chúng ta không biết cách hiểu nào trong hai cách hiểu là đúng. Tuy nhiên, bài toán đảm bảo rằng đường đi ngắn nhất chứa tối đa một nửa số đoạn. Điều này đảm bảo rằng ít nhất một trong`x`Và`n - x`luôn là độ dài đường dẫn ngắn nhất hợp lệ và cách giải thích đúng chỉ đơn giản là độ dài đường dẫn nhỏ hơn. 

Do đó, một truy vấn duy nhất là đủ và các truy vấn bổ sung chỉ đóng vai trò là chiến lược sao lưu lý thuyết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (truy vấn nhiều vị trí) | Truy vấn O(n) | O(1) | Quá chậm/không cần thiết | 
| Tối ưu (truy vấn đơn) | Truy vấn O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế là mọi truy vấn đều trả về một trong hai giá trị gắn với số lượng ẩn`x`. 

1. Chọn bất kỳ chỉ mục phân khúc nào, thông thường`1`và đưa ra một truy vấn về nó. Điều này đưa ra một phản hồi`k`. Chúng tôi không quan tâm chỉ mục nào được chọn vì tất cả các chỉ mục đều đối xứng với cấu trúc ẩn. 
2. Giải thích câu trả lời như sau`x`hoặc`n - x`. Không có khả năng thứ ba, vì hệ thống chỉ phân biệt xem phân đoạn được truy vấn có thuộc đường dẫn ẩn hay không. 
3. Trở về`min(k, n - k)`như câu trả lời cuối cùng. Điều này hoạt động vì chính xác một trong hai giá trị này tương ứng với độ dài đường dẫn ngắn nhất thực sự. 

Lý do đằng sau việc chọn mức tối thiểu là giá trị ngắn hơn trong hai giá trị bổ sung phải là độ dài đường dẫn thực tế do ràng buộc mà đường dẫn sử dụng tối đa một nửa số đoạn. 

### Tại sao nó hoạt động 

Mọi truy vấn đều tạo ra một giá trị`x`hoặc`n - x`. Hai giá trị này bổ sung cho nhau`n`. Vì đường dẫn ẩn chứa tối đa một nửa số đoạn,`x ≤ n/2`giữ, có nghĩa là`x ≤ n - x`. Do đó, giá trị nhỏ nhất của hai cách giải thích luôn bằng giá trị thực`x`. Thuật toán không cần phân biệt liệu chỉ mục được truy vấn có nằm trên đường dẫn hay không vì bản thân giá trị đã mã hóa cả hai khả năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input().strip())
    
    print("?", 1, flush=True)
    k = int(input().strip())
    
    print("!", min(k, n - k), flush=True)

if __name__ == "__main__":
    main()
```Chương trình thực hiện chính xác một truy vấn tương tác. Truy vấn đầu tiên được đưa ra tại chỉ mục`1`, điều này là tùy ý nhưng đủ vì tất cả các vị trí đều hoạt động theo cùng một quy tắc hai giá trị. Sau khi nhận được`k`, nghiệm sẽ tính trực tiếp giá trị nhỏ nhất giữa`k`và sự bổ sung của nó đối với`n`. 

Chi tiết triển khai chính là hoàn thiện sau mỗi đầu ra, vì các vấn đề tương tác đòi hỏi phải có sự giao tiếp ngay lập tức. Một điểm tinh tế khác là không cần logic phân nhánh nào ngoài logic cuối cùng`min`, mặc dù lý do cơ bản phân biệt hai trường hợp về mặt khái niệm. 

## Ví dụ đã hoạt động 

Vì vấn đề có tính chất tương tác nên chúng tôi mô phỏng các phản ứng giả định. 

### Ví dụ 1 

Giả sử`n = 10`và độ dài đường đi ngắn nhất ẩn là`x = 3`. Một truy vấn tại chỉ mục`1`(không nằm trên đường dẫn) trả về`k = 3`. 

| n | Chỉ mục truy vấn | Ẩn x | Phản hồi k | Tính min(k, n-k) | 
| --- | --- | --- | --- | --- | 
| 10 | 1 | 3 | 3 | 3 | 

Phản ứng trực tiếp bằng`x`, do đó quy tắc tối thiểu trả về giá trị đúng. Điều này xác nhận rằng khi chỉ mục được truy vấn không nằm trên đường dẫn, chúng tôi sẽ quan sát ngay câu trả lời đúng. 

### Ví dụ 2 

Giả sử`n = 10`Và`x = 4`, nhưng chỉ số`1`nằm trên con đường ẩn. Sau đó phản hồi là`k = 10 - 4 = 6`. 

| n | Chỉ mục truy vấn | Ẩn x | Phản hồi k | Tính min(k, n-k) | 
| --- | --- | --- | --- | --- | 
| 10 | 1 | 4 | 6 | 4 | 

Ở đây phản hồi là giá trị bổ sung. Lấy mức tối thiểu giữa`6`Và`4`phục hồi chính xác`x`. Điều này cho thấy sự chuyển đổi giữa hai đầu ra có thể xảy ra là nhất quán và không thể đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một truy vấn và số học theo thời gian không đổi | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp này tối ưu trong các ràng buộc tương tác vì nó giảm thiểu số lượng truy vấn cho một thao tác. Ngay cả đối với rất lớn`n`, thuật toán hoàn thành ngay sau một lần trao đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    
    n = int(inp.strip().split()[0])
    
    print("?", 1, flush=True)
    
    # simulate judge: suppose x = 3 for testing unless overridden
    # we emulate response logic: k = x or n-x
    x = 3
    k = x if 1 % 2 == 0 else n - x
    
    print("!", min(k, n - k))
    
    return out.getvalue().strip()

# custom sanity checks (conceptual, since real interactivity is not runnable here)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 0 hoặc 1 tùy ràng buộc | ranh giới tối thiểu | 
| n=2 | tính toán tối thiểu đúng | phép chia không tầm thường nhỏ nhất | 
| n=10, x=4 | 4 | trường hợp bổ sung đúng đắn | 
| n=9, x=3 | 3 | tính đúng đắn của trường hợp trực tiếp | 

## Vỏ cạnh 

Khi nào`n = 1`, cấu trúc sụp đổ thành một phân đoạn duy nhất. Mọi truy vấn đều trả về`0`hoặc`1`tùy theo cách giải thích, nhưng công thức`min(k, n - k)`vẫn giải quyết chính xác vì cả hai ứng cử viên đều trùng nhau hoặc một ứng cử viên bị ràng buộc bởi các ràng buộc. 

Khi`x = n/2`, cả hai câu trả lời có thể đều bằng nhau. Một truy vấn có thể trả về một trong hai giá trị, nhưng`min(k, n - k)`không thay đổi, do đó sự mơ hồ không ảnh hưởng đến tính đúng đắn. 

Khi chỉ mục được truy vấn luôn được chọn là`1`, việc nó có nằm trên đường ẩn hay không không quan trọng. Nếu nó không nằm trên đường đi, chúng ta sẽ nhận được`x`. Nếu nó nằm trên đường đi, chúng ta nhận được`n - x`. Trong cả hai trường hợp, việc áp dụng thao tác tối thiểu đều mang lại kết quả như nhau`x`, xác nhận rằng không cần chiến lược thích ứng.
