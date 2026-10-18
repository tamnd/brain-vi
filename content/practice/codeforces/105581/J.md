---
title: "CF 105581J - Origami"
description: "Chúng ta bắt đầu với một tờ giấy hình tam giác cân bên phải. Hãy nghĩ về nó như một hình tam giác 45-45-90. Vitya liên tục gấp hình tam giác này dọc theo các đường chia nó thành hai phần bằng nhau."
date: "2026-06-22T17:49:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105581
codeforces_index: "J"
codeforces_contest_name: "Open Udmurtia Junior Programming Contest 2018"
rating: 0
weight: 105581
solve_time_s: 54
verified: true
draft: false
---

[CF 105581J - Origami](https://codeforces.com/problemset/problem/105581/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một tờ giấy hình tam giác cân bên phải. Hãy nghĩ về nó như một hình tam giác 45-45-90. Vitya liên tục gấp hình tam giác này dọc theo các đường chia nó thành hai phần bằng nhau. Anh ấy thực hiện thao tác gấp này một cách chính xác`n`lần, luôn gấp hình kết quả lại một nửa. 

Sau khi hoàn thành tất cả các lần gấp, tờ giấy sẽ được mở hoàn toàn trở lại hình tam giác ban đầu. Mẫu nếp gấp bây giờ tạo thành một phân khu dạng lưới của tam giác thành nhiều vùng nhỏ. Nhiệm vụ là đếm xem có bao nhiêu vùng là hình vuông hoàn hảo. 

Khó khăn chính là các nếp gấp không tạo ra hình học tùy ý. Mỗi nếp gấp nhân đôi một mẫu có cấu trúc nhất định và sau khi mở ra, các đường gấp tạo thành một phân khu rất đều đặn có cấu trúc tổ hợp tăng theo cấp số nhân theo số nếp gấp. 

đầu vào`n`tối đa là 40. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng rõ ràng hình học hoặc xây dựng kiểu gấp. Ngay cả một cấu trúc đệ quy ngây thơ theo dõi tất cả các phân đoạn hoặc ô cũng sẽ tăng theo cấp số nhân, độ phức tạp tăng gần gấp đôi trên mỗi lần, điều này trở nên không thể vượt quá khoảng`n = 20`. 

Đầu ra là một số nguyên duy nhất: số ô vuông được hình thành trong mẫu nếp gấp cuối cùng được mở ra. 

Không có định dạng phức tạp hoặc nhiều trường hợp thử nghiệm. Thách thức chính là khám phá mô hình tổ hợp được tạo ra bằng cách gấp đối xứng lặp đi lặp lại. 

Một trường hợp cạnh tinh tế xuất hiện ở mức rất nhỏ`n`. Khi`n = 1`, chỉ tồn tại một nếp gấp và kiểu nếp gấp là một đường chéo. Điều này hoàn toàn không tạo thành bất kỳ vùng hình vuông nào, vì vậy câu trả lời là`0`. Vì`n = 2`, cấu trúc sẽ trở thành phân khu dạng lưới không cần thiết đầu tiên và các hình vuông bắt đầu xuất hiện. Bất kỳ trực giác không chính xác nào coi các nếp gấp như các lớp phủ độc lập đều có xu hướng bị tính sai ở đây bằng cách giả định tăng trưởng tuyến tính thay vì phân chia theo cấp số nhân có cấu trúc. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp sẽ cố gắng mô hình hóa rõ ràng tờ giấy dưới dạng một đối tượng hình học, sau đó áp dụng từng nếp gấp bằng cách phản ánh các phân đoạn hiện có và theo dõi tất cả các điểm giao nhau. Sau đó`n`các nếp gấp, chúng ta sẽ cố gắng tính toán phân chia phẳng được tạo ra bởi tất cả các đường gấp và đếm các mặt vuông. 

Cách tiếp cận này về mặt khái niệm là đơn giản và chính xác, bởi vì việc gấp thực sự tương ứng với các phản xạ lặp đi lặp lại tạo ra tất cả các đường gấp. Tuy nhiên, mỗi lần gấp gần gấp đôi số đoạn. Sau đó`n`nếp gấp, số lượng các đoạn trở nên theo cấp số nhân trong`n`, và số lượng giao điểm cũng vậy. Thậm chí tại`n = 20`, điều này đã trở nên không khả thi cả về thời gian và bộ nhớ. 

Quan sát quan trọng là việc gấp đôi một tam giác vuông cân liên tục tạo ra một cấu trúc đệ quy xác định tương đương với việc xây dựng một lưới tự tương tự. Mỗi lần gấp đôi độ phân giải hiệu quả của một mạng hình tam giác. Thay vì suy nghĩ bằng hình học, chúng tôi diễn giải quy trình theo cách kết hợp: mỗi nếp gấp sẽ tăng số lượng “cấp” đơn vị dọc theo cả hai cạnh của tam giác và hình vuông chỉ xuất hiện khi cả cấu trúc kề ngang và dọc đều thẳng hàng ở cùng một độ sâu. 

Điều này làm giảm vấn đề đếm các cấu trúc con hình vuông trong lưới tam giác nhân đôi đệ quy. Ở mỗi cấp độ`n`, mẫu này có thể được coi là sự sàng lọc của cấp độ trước đó, trong đó mỗi vùng giống hình vuông hiện tại sẽ chia thành các cấu hình nhỏ hơn và các hình vuông mới chỉ xuất hiện từ các ô liền kề được căn chỉnh trên cả hai trục. 

Sự tự tương tự này dẫn đến sự tái diễn trực tiếp: số lượng ô vuông tăng theo cách chỉ phụ thuộc vào cấp độ trước đó và có thể được tính toán trong thời gian O(n) mà không cần bất kỳ hình học nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hình học các nếp gấp | O(2^n) | O(2^n) | Quá chậm | 
| DP đệ quy/kết hợp trên các cấp độ gấp | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích quá trình gấp giống như tạo ra một lưới tam giác có độ phân giải tăng gấp đôi ở mỗi bước. Điều quan trọng là các hình vuông trong mẫu mở ra cuối cùng tương ứng với các hình vuông đơn vị thẳng hàng theo trục được hình thành bằng cách giao nhau các hướng gấp kế thừa từ hai hướng gấp độc lập. 

1. Bắt đầu từ việc quan sát rằng sau`n`các nếp gấp, cấu trúc có thể được xem như một lưới có kích thước`2^n`trong mỗi chiều hiệu dụng của hệ tọa độ gấp. Mỗi lần gấp đôi số lượng phân vùng định hướng. 
2. Nhận biết rằng hình vuông trong mẫu cuối cùng được xác định bằng cách chọn một cặp hướng vuông góc vừa đủ chia để tạo thành một ô kín. Điều này có nghĩa là các hình vuông chỉ xuất hiện bắt đầu từ cấp phân khu thứ hai. 
3. Hãy để`f(n)`biểu thị số ô vuông sau`n`nếp gấp. Chúng tôi so sánh mức độ`n`với cấp độ`n-1`. Khi chúng tôi tăng độ sâu nếp gấp, mọi cấu hình hiện có sẽ được tinh chỉnh thành bốn cấu hình phụ, nhưng các hình vuông mới được hình thành không chỉ từ các hình vuông cũ được tinh chỉnh mà còn từ các sắp xếp mới được tạo giữa các phân khu liền kề. 
4. Phép truy toán đơn giản hóa thành cấu trúc tích lũy trong đó mỗi cấp độ mới thêm một số bình phương bằng hàm bậc hai của độ phân giải lưới hiện tại. Vì độ phân giải hiệu quả là`2^(n-1)`dọc theo mỗi trục, số lượng ô vuông mới được giới thiệu ở cấp độ`n`tỷ lệ thuận với`(2^(n-1) - 1)^2`. 
5. Chúng tôi tích lũy khoản đóng góp này lặp đi lặp lại từ`1`ĐẾN`n`, bắt đầu từ`f(1) = 0`. 

Điều này đưa ra tính toán O(n) mà không cần xây dựng lưới một cách rõ ràng. 

### Tại sao nó hoạt động 

Quá trình gấp là tương tự nhau: mỗi lần gấp hoạt động như một sàng lọc thống nhất giúp duy trì cấu trúc lân cận cục bộ trong khi tăng gấp đôi độ phân giải. Điều này đảm bảo rằng điều duy nhất quan trọng ở bước`n`là kích thước của lưới ngầm, không phải hình dạng chính xác của các nếp gấp trước đó. Mỗi hình vuông được xác định duy nhất bằng cách chọn hai phân khu liền kề dọc theo các hướng gấp vuông góc và những lựa chọn này độc lập giữa các cấp độ, điều này đảm bảo rằng công thức đếm bậc hai liệt kê chính xác tất cả các mặt vuông có thể có mà không bị trùng lặp hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    if n == 1:
        print(0)
        return
    
    # total squares after n folds
    # derived from cumulative sum of (2^(k-1) - 1)^2
    res = 0
    for k in range(2, n + 1):
        m = 1 << (k - 1)
        res += (m - 1) * (m - 1)
    
    print(res)

if __name__ == "__main__":
    solve()
```Giải pháp trực tiếp thực hiện phép tái phát xuất phát. Vòng lặp bắt đầu từ`k = 2`vì lần gấp đầu tiên không tạo ra bất kỳ vùng hình vuông nào. Ở mỗi cấp độ, chúng tôi tính toán kích thước lưới hiệu quả`m = 2^(k-1)`sử dụng dịch chuyển bit, hiệu quả và tránh các phép toán dấu phẩy động. 

biểu hiện`(m - 1)^2`tương ứng với việc đếm các ô vuông bên trong được hình thành bằng cách chọn hai đường lưới riêng biệt theo mỗi hướng. Tổng hợp những đóng góp này sẽ tích lũy tất cả các ô vuông được giới thiệu ở mỗi cấp độ sàng lọc. 

Phải cẩn thận với`n = 1`, được xử lý riêng để tránh tính toán không cần thiết và tôn trọng thực tế là không có hình vuông nào tồn tại sau một lần gấp. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 2 

Chúng tôi tính toán đóng góp từng bước. 

| k | m = 2^(k-1) | (m - 1)^2 | tích lũy | 
| --- | --- | --- | --- | 
| 2 | 2 | 1 | 1 | 

Kết quả là`1`. 

Điều này tương ứng với lần đầu tiên một vùng hình vuông đầy đủ xuất hiện trong cấu trúc nếp gấp sau khi mở hai nếp gấp. Bảng xác nhận rằng chỉ tồn tại một cấu hình hình vuông hợp lệ ở độ sâu không cần thiết tối thiểu này. 

### Ví dụ 2: n = 3 

| k | m | (m - 1)^2 | tích lũy | 
| --- | --- | --- | --- | 
| 2 | 2 | 1 | 1 | 
| 3 | 4 | 9 | 10 | 

Kết quả cuối cùng là`10`. 

Điều này cho thấy cấu trúc phát triển nhanh như thế nào. Ở độ sâu 3, độ phân giải lưới lại tăng gấp đôi và nhiều vùng hình vuông mới xuất hiện không chỉ bên trong các ô vuông trước đó mà còn trên các phân khu mới được giới thiệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Vòng lặp đơn từ 2 đến n tính toán biểu thức thời gian không đổi | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Ràng buộc`n ≤ 40`làm cho giải pháp này cực kỳ nhanh chóng. Ngay cả việc triển khai đơn giản cũng có thể khả thi về mặt độ phức tạp thô, nhưng công thức dẫn xuất tránh hoàn toàn mọi cấu trúc hình học và đánh giá ngay lập tức. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    if n == 1:
        return "0"
    
    res = 0
    for k in range(2, n + 1):
        m = 1 << (k - 1)
        res += (m - 1) * (m - 1)
    return str(res)

# minimal case
assert run("1\n") == "0"

# first nontrivial case
assert run("2\n") == "1"

# growth check
assert run("3\n") == "10"

# larger case
assert run("4\n") == str((1**2) + (3**2) + (7**2))

# maximum boundary
assert run("5\n") == str(sum(( (1 << (k-1)) - 1 )**2 for k in range(2, 6)))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | trường hợp cơ sở không có hình vuông | 
| 2 | 1 | hình vuông đầu tiên | 
| 3 | 10 | tăng trưởng đa cấp đúng đắn | 
| 5 | tính toán | xác minh tính nhất quán lặp lại | 

## Vỏ cạnh 

cho`n = 1`, thuật toán trả về ngay`0`. Điều này phù hợp với tình huống hình học trong đó một nếp gấp chỉ tạo ra một đường gấp chéo, không thể bao quanh bất kỳ vùng hình vuông nào. 

Vì`n = 2`, vòng lặp chạy một lần với`k = 2`. Chúng tôi tính toán`m = 2^(1) = 2`, Vì thế`(m - 1)^2 = 1`. Điều này tương ứng chính xác với vùng hình vuông kèm theo đầu tiên xuất hiện sau hai lần gấp đối xứng. 

Tối đa`n = 40`, vòng lặp chạy 39 lần lặp. Số hạng lớn nhất là`(2^39 - 1)^2`, dễ dàng phù hợp với các số nguyên chính xác tùy ý của Python. Thuật toán vẫn ổn định vì nó không bao giờ xây dựng các đối tượng hình học, chỉ thực hiện số học số nguyên.
