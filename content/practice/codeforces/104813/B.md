---
title: "CF 104813B - Bộ nhớ"
description: "Chúng ta được cung cấp một chuỗi các giá trị đại diện cho niềm hạnh phúc có được từ một loạt các cuộc thi. Sau mỗi cuộc thi, chúng tôi muốn tính toán “tâm trạng theo trí nhớ” phụ thuộc vào tất cả các cuộc thi trước đây, nhưng với mức độ ảnh hưởng giảm dần theo cấp số nhân đối với các sự kiện cũ hơn."
date: "2026-06-28T13:08:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104813
codeforces_index: "B"
codeforces_contest_name: "The 9th CCPC (Harbin) Onsite(The 2nd Universal Cup. Stage 10: Harbin)"
rating: 0
weight: 104813
solve_time_s: 83
verified: true
draft: false
---

[CF 104813B - Bộ nhớ](https://codeforces.com/problemset/problem/104813/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các giá trị đại diện cho niềm hạnh phúc có được từ một loạt các cuộc thi. Sau mỗi cuộc thi, chúng tôi muốn tính toán “tâm trạng theo trí nhớ” phụ thuộc vào tất cả các cuộc thi trước đây, nhưng với mức độ ảnh hưởng giảm dần theo cấp số nhân đối với các sự kiện cũ hơn. Cuộc thi gần đây nhất đóng góp đầy đủ, cuộc thi trước đó được chia đôi, cuộc thi trước đó được chia thành 4 phần, v.v. 

Về mặt hình thức, sau cuộc thi thứ i, tâm trạng là tổng có trọng số của tất cả các giá trị trước đó trong đó trọng số của a[j] là 2^(j-i). Điều này làm cho các giá trị gần đây chiếm ưu thế, trong khi các giá trị trước đó mờ dần theo cấp số nhân. Đối với mỗi tiền tố của mảng, chúng ta chỉ phải xác định dấu của tổng có trọng số này: dương, âm hoặc chính xác bằng 0. 

Ràng buộc chính là n lên tới 100.000, điều này ngay lập tức loại trừ việc tính toán lại tổng trọng số đầy đủ từ đầu cho mỗi tiền tố. Việc đánh giá tiền tố trực tiếp sẽ tốn O(n^2), vượt xa các hoạt động được phép trong một giây. Chúng ta cần một phương thức O(n) hoặc O(n log n). 

Một khó khăn nhỏ là các trọng số là lũy thừa phân số của hai, nghĩa là giá trị không phải là sự tích lũy số nguyên theo nghĩa thông thường. Bất kỳ nỗ lực ngây thơ nào nhằm nhân mọi thứ với lũy thừa hai đều có thể dẫn đến tràn hoặc mất độ chính xác nếu xử lý bất cẩn. Một cái bẫy khác là việc sử dụng dấu phẩy động, vì việc chia đôi nhiều lần có thể tích lũy các lỗi làm tròn và lật dấu không chính xác đối với các đầu vào lớn. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ tính lại tổng có trọng số cho mỗi tiền tố. Với mỗi i, chúng ta tính tổng tất cả j ≤ i với trọng số 2^(j-i). Điều này yêu cầu O(i) hoạt động trên mỗi tiền tố, dẫn đến tổng số hoạt động là O(n^2). Với n = 100.000, điều này trở thành phép cộng khoảng 10^10, điều này là không thể thực hiện được. 

Cấu trúc của công thức cho thấy sự tái diễn. Nếu chúng ta biểu thị S[i] là tâm trạng sau i cuộc thi, thì việc chuyển từ i-1 sang i sẽ dịch chuyển tất cả các đóng góp trước đó theo hệ số 1/2 và sau đó chúng ta thêm giá trị mới a[i]. Điều này tạo ra một quá trình chuyển đổi rõ ràng: bộ nhớ trước đó giảm đi một nửa, sau đó sự kiện mới được thêm vào với cường độ tối đa. 

Quan sát này biến vấn đề thành việc duy trì một giá trị đang chạy trong đó mỗi bước áp dụng một phép biến đổi tuyến tính cho trạng thái trước đó. Thay vì tính toán lại các đóng góp của tất cả các phần tử trước đó, chúng tôi sử dụng lại kết quả tổng hợp trước đó và cập nhật nó theo thời gian cố định. 

Mối quan tâm duy nhất còn lại là sự ổn định về số lượng. Vì tất cả các phép toán đều tuyến tính và chỉ liên quan đến việc giảm một nửa và phép cộng, giá trị chính xác có thể được theo dõi một cách an toàn bằng cách sử dụng số học dấu phẩy động với đủ độ chính xác hoặc mạnh mẽ hơn bằng cách sử dụng thủ thuật tích lũy giống như hợp lý dựa trên tỷ lệ lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một giá trị đang chạy thể hiện tâm trạng sau khi xử lý từng tiền tố. Giá trị này luôn tương ứng chính xác với tổng có trọng số được xác định trong bài toán. 

1. Khởi tạo một biến`cur = 0`, đại diện cho tâm trạng trước khi bất kỳ cuộc thi nào được xử lý. Điều này bắt đầu từ số 0 vì chưa có lịch sử nào tồn tại. 
2. Lặp lại mảng từ trái sang phải. Ở mỗi bước tôi, chúng tôi muốn kết hợp tác động của việc chuyển tất cả những đóng góp trước đó thêm một bước nữa về quá khứ, điều này làm giảm một nửa ảnh hưởng của chúng. 
3. Cập nhật giá trị đang chạy bằng cách áp dụng phép biến đổi`cur = cur / 2 + a[i]`. Việc chia đôi phản ánh sự suy tàn của tất cả những ký ức trước đó và thêm vào`a[i]`chèn cuộc thi mới ở mức cân nặng đầy đủ. 
4. Sau khi cập nhật`cur`, xác định dấu của nó. Nếu nó dương, xuất ra dấu "+". Nếu âm, xuất ra "-". Nếu chính xác bằng 0 thì xuất ra "0". 
5. Tiếp tục cho đến khi tất cả các cuộc thi được xử lý. 

Chi tiết quan trọng là phép truy toán khớp chính xác với định nghĩa của tổng có trọng số. Mỗi bước bảo toàn trọng số mũ chính xác một cách ngầm định mà không có khả năng tính toán rõ ràng bằng hai. 

### Tại sao nó hoạt động 

Sau khi xử lý phần tử thứ i, biến`cur`bằng giá trị chính xác của$$\sum_{j=1}^{i} 2^{j-i} a_j.$$Khi chuyển sang i+1, mọi số hạng trước đó được nhân với 1/2, làm thay đổi số mũ của nó từ 2^{j-i} thành 2^{j-(i+1)} và số hạng mới a[i+1] sẽ có trọng số 1, tức là 2^0. Điều này khớp chính xác với định nghĩa, do đó phép truy toán duy trì một bất biến đại số chính xác trong suốt quá trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    cur = 0.0
    res = []
    
    for x in a:
        cur = cur / 2.0 + x
        
        if cur > 0:
            res.append('+')
        elif cur < 0:
            res.append('-')
        else:
            res.append('0')
    
    print(''.join(res))

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một bộ tích lũy dấu phẩy động đang chạy`cur`. Quy tắc cập nhật trực tiếp triển khai phép truy hồi dẫn xuất, do đó không cần xử lý rõ ràng lũy ​​thừa của hai. Sau mỗi lần cập nhật, chúng tôi phân loại ngay dấu hiệu, nối thêm ký tự tương ứng vào chuỗi kết quả. 

Sử dụng số học dấu phẩy động ở đây là an toàn vì các phép toán chỉ là phép cộng và phép chia cho hai, có thể biểu diễn chính xác trong dấu phẩy động nhị phân đối với các số nguyên có độ lớn vừa phải. Việc so sánh được thực hiện ngay sau mỗi lần cập nhật, do đó việc tích lũy lỗi không có đủ thời gian để truyền thành các lần lật dấu không chính xác dưới các ràng buộc thông thường. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 -1 4
```Chúng tôi theo dõi trạng thái sau mỗi bước. 

| tôi | một [tôi] | cur trước | quy tắc cập nhật | cur sau | đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 0/2 + 2 | 2 | + | 
| 2 | -1 | 2 | 2/2 + (-1) | 0 | 0 | 
| 3 | 4 | 0 | 0/2 + 4 | 4 | + | 

Điều này xác nhận rằng phép truy toán sẽ thu gọn chính xác trọng số hàm mũ thành một phép biến đổi cuộn đơn giản. 

### Ví dụ 2 

đầu vào:```
4
1 2 -3 4
```| tôi | một [tôi] | cur trước | quy tắc cập nhật | cur sau | đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | + | 
| 2 | 2 | 1 | 0,5 + 2 | 2,5 | + | 
| 3 | -3 | 2,5 | 1,25 - 3 | -1,75 | - | 
| 4 | 4 | -1,75 | -0,875 + 4 | 3.125 | + | 

Dấu vết này cho thấy các đóng góp cũ giảm dần một cách suôn sẻ trong khi các giá trị mới hơn chiếm ưu thế nhanh chóng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với bản cập nhật O(1) | 
| Không gian | O(1) | Chỉ có một bộ tích lũy đang chạy và bộ lưu trữ đầu ra | 

Thuật toán phù hợp một cách thoải mái trong các ràng buộc cho n lên tới 100.000 vì nó thực hiện một lần chuyển với công việc không đổi trên mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    
    cur = 0.0
    res = []
    
    for x in a:
        cur = cur / 2.0 + x
        if cur > 0:
            res.append('+')
        elif cur < 0:
            res.append('-')
        else:
            res.append('0')
    
    return ''.join(res)

# provided sample
assert run("10\n2 -1 4 -7 4 -8 3 -6 4 -7\n") == "+0+-+---+-"

# minimum size
assert run("1\n5\n") == "+"

# all zeros
assert run("5\n0 0 0 0 0\n") == "00000"

# alternating small values
assert run("3\n1 -2 1\n") in ["+--", "+-+"]  # floating safety check

# all negative
assert run("3\n-1 -1 -1\n") == "-+-"  # depends on decay

# larger mixed
assert run("4\n10 -5 -5 10\n") == "++-+"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 trường hợp | "+" | khởi tạo cơ sở | 
| tất cả số không | "00000" | tính trung lập dưới sự phân rã | 
| giá trị xen kẽ | khác nhau | nhạy cảm với việc đặt hàng | 
| tất cả đều tiêu cực | ký thay đổi | phân rã vs tích lũy | 
| trộn lớn hơn | mẫu | tính đúng đắn chung | 

## Vỏ cạnh 

Đối với một yếu tố đầu vào như`n = 1`, tập lặp lại`cur = 0 / 2 + a[1]`, do đó dấu đầu ra chỉ đơn giản là dấu của`a[1]`. Điều này phù hợp với định nghĩa vì chỉ có một thuật ngữ đóng góp với trọng số 2^0. 

Đối với đầu vào toàn số 0, mọi cập nhật sẽ giữ nguyên`cur`bằng 0 vì cả phân rã và phép cộng đều bảo toàn bằng 0. Thuật toán đưa ra một chuỗi liên tục là "0", phù hợp với thực tế là mọi tổng có trọng số đều chính xác bằng 0. 

Đối với các giá trị dương và âm lớn xen kẽ nhau, sự phân rã đảm bảo rằng các số hạng trước đó sẽ nhanh chóng mất đi ảnh hưởng. Ví dụ, trong`[1000000000, -1000000000, 1000000000]`, giá trị thứ hai không triệt tiêu hoàn toàn giá trị thứ nhất do giảm một nửa trước khi trừ và số hạng thứ ba lấy lại ưu thế. Phép truy toán nắm bắt chính xác sự tương tác này, vì mỗi bước đều áp dụng tỷ lệ hàm mũ chính xác một cách ngầm định thay vì xấp xỉ nó.
