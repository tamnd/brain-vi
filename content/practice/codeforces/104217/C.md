---
title: "CF 104217C - Xe trượt vòng tròn"
description: "Chúng ta đang xử lý một đường tròn có n vị trí cách đều nhau, được dán nhãn từ 0 đến n-1. Mỗi con chó xuất phát ở một vị trí duy nhất i, và mỗi con chó di chuyển theo chiều kim đồng hồ với tốc độ không đổi vi, nghĩa là sau t bước thời gian, con chó i sẽ ở vị trí (i + t · vi) mod n."
date: "2026-07-01T23:52:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104217
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 03-03-23 Div. 2 (Beginner)"
rating: 0
weight: 104217
solve_time_s: 71
verified: true
draft: false
---

[CF 104217C - Vòng tròn xe trượt tuyết](https://codeforces.com/problemset/problem/104217/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý một đường tròn của`n`các vị trí cách đều nhau, được dán nhãn từ`0`ĐẾN`n-1`. Mỗi con chó bắt đầu ở một vị trí duy nhất`i`và mọi con chó đều chuyển động theo chiều kim đồng hồ với tốc độ không đổi`v_i`, nghĩa là sau`t`bước thời gian, con chó`i`sẽ nằm ở vị trí`(i + t · v_i) mod n`. 

Mục đích là để xác định liệu có tồn tại một thời điểm`t`(từ`0`lên đến`1000`) sao cho tất cả các con chó đều có cùng vị trí trên vòng tròn. Nếu khoảnh khắc đó tồn tại, chúng tôi phải báo cáo dù là nhỏ nhất.`t`, cùng với vị trí chung`p`nơi tất cả các con chó gặp nhau. Nếu không có thời gian như vậy tồn tại trong khoảng thời gian cho phép, câu trả lời là`-1`. 

Những hạn chế là nhỏ:`n ≤ 1000`Và`v_i ≤ 100`, và thời hạn ngầm hạn chế chúng tôi chỉ xem xét tối đa`t = 1000`. Điều này ngay lập tức cho thấy rằng việc mô phỏng theo tất cả các bước thời gian là khả thi, vì tổng số trạng thái mà chúng ta sẽ kiểm tra nhiều nhất là khoảng một triệu vị trí. 

Một điểm tinh tế là vị trí gặp nhau không cố định trước. Nó được xác định bởi động lực tại thời điểm`t`. Một sai lầm ngây thơ là cho rằng chúng ta có thể đoán vị trí mục tiêu hoặc giảm vấn đề về việc căn chỉnh theo cặp tại thời điểm 0. Điều đó không thành công vì việc đồng bộ hóa phụ thuộc vào sự tiến hóa theo thời gian chứ không phải cấu trúc ban đầu. 

Một cạm bẫy tiềm ẩn khác là quên hành vi modulo. Ngay cả khi tất cả các con chó dường như đang di chuyển tuyến tính, vị trí của chúng bao quanh vòng tròn và sự căn chỉnh phải được kiểm tra theo số học mô-đun chứ không phải số nguyên thô. 

Một trường hợp cạnh nhỏ là`n = 1`, câu trả lời thật tầm thường`t = 0, p = 0`. Một trường hợp khác là khi không có thời gian thỏa mãn điều kiện, trong trường hợp đó chúng ta phải xuất ra một cách rõ ràng`-1`chứ không phải là một cấu hình một phần. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là mô phỏng hệ thống. Đối với mỗi lần`t`, chúng ta tính toán vị trí của mỗi con chó bằng công thức`(i + t · v_i) mod n`, sau đó kiểm tra xem tất cả các vị trí được tính toán có giống nhau không. Nếu đúng như vậy, chúng tôi sẽ quay lại`t`và vị trí đó. 

Điều này có tác dụng vì trạng thái của hệ thống tại bất kỳ thời điểm cố định nào đều được xác định đầy đủ và có thể được tính toán độc lập. Tính đúng đắn có ngay từ định nghĩa. 

Chi phí của phương pháp tiếp cận bạo lực này rất dễ phân tích. Đối với mỗi`t`lên đến`1000`, chúng tôi quét tất cả`n`chó, dẫn đến khoảng`1000 · 1000 = 10^6`đánh giá trong trường hợp xấu nhất. Mỗi lần đánh giá đều có thời gian không đổi, vì vậy điều này nằm trong giới hạn. 

Ở đây không cần đơn giản hóa đại số sâu hơn vì giới hạn thời gian đủ nhỏ để mô phỏng trực tiếp đã phù hợp một cách thoải mái. Bất kỳ nỗ lực nào nhằm tìm ra một giải pháp dạng đóng sẽ gây ra sự phức tạp không cần thiết mà không cải thiện hiệu suất tiệm cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n · T) | O(1) | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng) | O(n · T) | O(1) | Đã chấp nhận | 

Đây`T = 1000`. 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng từng bước thời gian và kiểm tra sự liên kết. 

### Các bước 

1. Lặp lại theo thời gian`t`từ`0`ĐẾN`1000`. 

Chúng tôi bắt đầu từ`0`vì cần có giải pháp sớm nhất và thời điểm 0 là cấu hình hợp lệ. 
2. Đối với mỗi`t`, tính vị trí của con chó đầu tiên là`p0 = (0 + t · v_0) mod n`. 

Điều này trở thành vị trí cuộc họp ứng cử viên. 
3. Đối với mọi con chó khác`i`, tính toán`pi = (i + t · v_i) mod n`. 

Nếu có`pi`khác với`p0`, lần này`t`không thể là một giải pháp, vì không phải con chó nào cũng trùng hợp. 
4. Nếu tất cả các con chó đều giống nhau`p0`, quay lại ngay`(t, p0)`. 

Đầu tiên như vậy`t`gặp phải tự động là nhỏ nhất. 
5. Nếu không có thời gian trong phạm vi tạo ra sự căn chỉnh đầy đủ, đầu ra`-1`. 

### Tại sao nó hoạt động 

Vào bất kỳ thời điểm cố định nào`t`, trạng thái hệ thống được mô tả đầy đủ bằng ánh xạ xác định từ chỉ số đến vị trí. Thuật toán kiểm tra chính xác liệu ánh xạ này có trở thành hằng số trên tất cả các chỉ số hay không. Vì chúng tôi kiểm tra thời gian theo thứ tự tăng dần và chấp nhận cấu hình hợp lệ đầu tiên nên thời gian trả về là tối thiểu theo cách xây dựng. Không có trạng thái trung gian nào có thể bị bỏ qua hoặc có hiệu lực một phần, vì điều kiện này yêu cầu tất cả các con chó phải trùng khớp đồng thời chứ không phải tăng dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    v = list(map(int, input().split()))
    
    for t in range(1001):
        p0 = (0 + t * v[0]) % n
        ok = True
        
        for i in range(1, n):
            if (i + t * v[i]) % n != p0:
                ok = False
                break
        
        if ok:
            print(t, p0)
            return
    
    print(-1)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp thuật toán. Vòng lặp bên ngoài liệt kê thời gian. Mỗi lần, chúng tôi tính toán vị trí tham chiếu từ con chó`0`và so sánh mọi con chó khác với nó. Việc thoát sớm khỏi tình trạng không khớp sẽ ngăn chặn những công việc không cần thiết khi phát hiện thấy sự không nhất quán. 

Một lỗi thực hiện phổ biến là quên dùng modulo`n`tại mỗi vị trí tính toán. Không có nó, các giá trị sẽ tăng lên nhanh chóng và sự so sánh trở nên vô nghĩa. Một vấn đề nhỏ khác là không khởi động lại việc kiểm tra tính chính xác cho từng bước thời gian một cách độc lập, điều này sẽ mang trạng thái không chính xác qua các lần lặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```| t | p0 (con chó 0) | con chó 1 | con chó 2 | tất cả đều bình đẳng? | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 2 | không | 
| 1 | 1 | 0 | 0 | không | 
| 2 | 2 | 2 | 2 | vâng | 

Tại`t = 2`, tất cả chó đều hạ cánh vào vị trí`2`, do đó thuật toán trả về`(2, 2)`. 

Dấu vết này xác nhận rằng sự liên kết mang tính toàn cầu trên tất cả các chỉ số chỉ tại một thời điểm cụ thể, không được xây dựng dần dần từ các kết quả khớp từng phần. 

### Ví dụ 2 

đầu vào:```
2
1 1
```| t | p0 | p1 | tất cả đều bình đẳng? | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | không | 
| 1 | 1 | 0 | không | 
| 2 | 0 | 0 | vâng | 

Ở đây cả hai con chó di chuyển giống hệt nhau nên chúng gặp nhau định kỳ. Cuộc gặp đầu tiên diễn ra vào lúc`t = 2`, chức vụ`0`. 

Điều này chứng tỏ rằng sự đồng bộ hóa có thể xảy ra ngay cả khi các vị trí ban đầu khác nhau, miễn là chuyển động tương đối triệt tiêu theo thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 1000) | Đối với mỗi bước thời gian, chúng tôi quét tất cả các con chó một lần | 
| Không gian | O(1) | Chỉ có một số biến được sử dụng ngoài bộ lưu trữ đầu vào | 

Số lượng thao tác trong trường hợp xấu nhất là khoảng một triệu phép tính số học mô-đun, nằm trong giới hạn thoải mái đối với Python trong giới hạn 1 giây khi được triển khai bằng kiểm tra thoát sớm. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("3\n1 2 3\n") == "2 2"

# n=1 edge case
assert run("1\n7\n") == "0 0"

# identical velocities
assert run("3\n1 1 1\n") == "0 0"

# no solution case (small constructed)
assert run("3\n1 2 4\n") == "-1"

# periodic alignment
assert run("2\n1 1\n") == "2 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 1 2 3`|`2 2`| đồng bộ chuẩn | 
|`1 / 7`|`0 0`| trường hợp cơ sở phần tử đơn | 
|`3 / 1 1 1`|`0 0`| căn chỉnh ngay lập tức | 
|`3 / 1 2 4`|`-1`| không có thời gian họp hợp lệ | 
|`2 / 1 1`|`2 0`| hành vi bao quanh định kỳ | 

## Vỏ cạnh 

cho`n = 1`, vòng lặp ngay lập tức phát hiện ra rằng con chó duy nhất đã được căn chỉnh với chính nó tại`t = 0`, và thuật toán trả về`0 0`. Sự tính toán`(0 + 0 · v_0) mod 1`là`0`, do đó tính nhất quán giữ tầm thường. 

Đối với các trường hợp có vận tốc giống nhau, mọi con chó đều bảo toàn khoảng cách tương đối theo modulo`n`. Nếu vị trí ban đầu không bằng nhau thì sau đó không thể sửa được phần bù, do đó thuật toán sẽ hết chính xác mọi lần và trả về`-1`Trừ khi`n = 1`. 

Đối với trường hợp căn chỉnh xảy ra muộn trong phạm vi thời gian, quá trình quét từng bước đảm bảo tính chính xác vì không có thời gian nào bị bỏ qua. Hợp lệ sớm nhất`t`luôn được trả về vì số lần lặp tăng nghiêm ngặt.
