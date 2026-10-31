---
title: "CF 106298G - Một vấn đề về chuỗi nhị phân khác"
description: "Chúng tôi đang làm việc với các chuỗi nhị phân trong đó cấu trúc quan trọng không phải là các ký tự riêng lẻ mà là các phân đoạn liền kề của các ký tự bằng nhau. Mỗi đoạn tối đa của các số 0 liên tiếp hoặc các số 1 liên tiếp tạo thành một khối."
date: "2026-06-19T16:50:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106298
codeforces_index: "G"
codeforces_contest_name: "OCPC 2024 Summer, Day 4: wuhudsm Contest"
rating: 0
weight: 106298
solve_time_s: 54
verified: true
draft: false
---

[CF 106298G - Tuy nhiên, một vấn đề về chuỗi nhị phân khác](https://codeforces.com/problemset/problem/106298/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với các chuỗi nhị phân trong đó cấu trúc quan trọng không phải là các ký tự riêng lẻ mà là các phân đoạn liền kề của các ký tự bằng nhau. Mỗi đoạn cực đại của liên tiếp`0`s hoặc liên tiếp`1`s tạo thành một khối. Ví dụ, chuỗi`0001110011`gồm bốn khối:`000`,`111`,`00`, Và`11`. 

Quy tắc chuyển đổi được phép được mô tả theo các khối này. Một khối không được phép biến mất hoàn toàn nhưng độ dài của nó có thể linh hoạt. Một khối có thể được kéo dài hoặc thu nhỏ tùy ý, nghĩa là chúng ta có thể thay đổi số ký tự chứa trong đó, nhưng chúng ta không thể xóa khối hoặc hợp nhất khối đó với các khối lân cận. Điều quan trọng là trình tự các khối và giá trị bắt đầu của chúng. 

Điều kiện để chuyển đổi chuỗi`a`thành chuỗi`b`giảm để so sánh cấu trúc khối của chúng. Nếu cả hai chuỗi có cùng số khối và khối đầu tiên có cùng giá trị thì chuỗi này có thể được chuyển đổi thành chuỗi kia. 

Do đó, đầu vào bao gồm hai chuỗi nhị phân cho mỗi trường hợp thử nghiệm và đầu ra là quyết định xem có thể chuyển đổi theo quy tắc bảo toàn khối hay không. 

Các ràng buộc không được đưa ra rõ ràng trong đoạn mã câu lệnh, nhưng các vấn đề ở dạng này thường cho phép các chuỗi lên tới khoảng 10^5 cho mỗi trường hợp thử nghiệm trở lên. Điều đó ngay lập tức loại trừ mọi giải pháp mô phỏng các phép biến đổi hoặc cố gắng xây dựng các chuỗi trung gian. Bất kỳ cách tiếp cận nào cũng phải giảm từng chuỗi thành quét tuyến tính và so sánh hằng số hoặc logarit cho mỗi trường hợp thử nghiệm. 

Một sai lầm ngây thơ là so sánh trực tiếp các chuỗi hoặc chỉ so sánh số lượng`0`cát`1`S. Cả hai đều thất bại vì vấn đề cấu trúc. Ví dụ,`0011`Và`0101`có cùng số số 0 và số 1 nhưng cấu trúc khối khác nhau và không có chuỗi thao tác nào được phép có thể dung hòa được cấu trúc xen kẽ. 

Một trường hợp phức tạp khác là chuỗi ký tự đơn. Ví dụ,`0`có thể chuyển đổi thành`0000`, Nhưng`0`không thể chuyển đổi thành`1`ngay cả khi cả hai đều có độ dài bằng một, vì khối bắt đầu khác nhau. 

## Phương pháp tiếp cận 

Cách mạnh mẽ để nghĩ về điều này là mô phỏng tất cả các cách có thể thay đổi kích thước khối trong chuỗi`a`và cố gắng khớp chuỗi`b`. Vì mỗi khối có thể tăng hoặc giảm tự do nhưng phải giữ nguyên vị trí, trạng thái có ý nghĩa duy nhất là phân vùng chuỗi thành các khối. Một mô phỏng brute-force sẽ cố gắng gán độ dài cho mỗi khối`a`để phù hợp`b`. Trong trường hợp xấu nhất, nếu cả hai chuỗi có n ký tự, thì sẽ có các khối O(n) và việc thử tất cả các phép gán hoặc sắp xếp sẽ suy biến thành logic khớp hàm mũ hoặc ít nhất là bậc hai do sự phụ thuộc giữa các khối liền kề. Điều này nhanh chóng trở nên không khả thi ngay cả với n khoảng 10^4. 

Quan sát quan trọng là ranh giới khối không bao giờ thay đổi. Vì các khối không thể bị phá hủy hoặc hợp nhất nên mọi phép biến đổi hợp lệ sẽ bảo toàn chuỗi chính xác của các loại khối. Sự tự do duy nhất nằm trong chiều dài của mỗi khối nhà. Điều đó có nghĩa là toàn bộ vấn đề chuyển đổi được chuyển thành kiểm tra sự tương đương về cấu trúc giữa hai mã hóa độ dài chạy: so sánh xem chuỗi ký tự xen kẽ có giống nhau hay không. 

Điều này làm giảm vấn đề khi tính toán mã hóa độ dài chạy của cả hai chuỗi và kiểm tra xem cả hai chuỗi có khớp chính xác cả về độ dài và ký tự hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ / O(n²) | O(n) | Quá chậm | 
| Tối ưu (so sánh RLE) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chuỗi bằng cách nén nó thành chuỗi khối. 

1. Chuỗi quét`a`từ trái sang phải và xây dựng mã hóa độ dài chạy của nó. Mỗi khi ký tự hiện tại khác với ký tự trước đó, chúng ta sẽ đóng một khối và bắt đầu một khối mới. Chúng tôi chỉ lưu trữ ký tự của mỗi khối chứ không phải độ dài của nó vì độ dài không liên quan đến việc chuyển đổi. 
2. Lặp lại quá trình tương tự cho chuỗi`b`, tạo ra chuỗi khối của nó. 
3. So sánh số khối trong cả hai chuỗi. Nếu chúng khác nhau thì không có cách nào để căn chỉnh cấu trúc khối mà không hợp nhất hoặc chia tách, điều này bị cấm. 
4. So sánh ký tự bắt đầu của cả hai chuỗi. Vì khối đầu tiên phải khớp về giá trị nên việc không khớp ở đây ngay lập tức khiến việc chuyển đổi không thể thực hiện được. 
5. So sánh từng khối theo vị trí. Mỗi khối ở vị trí i của`a`phải có cùng ký tự với khối tương ứng trong`b`. Nếu có bất kỳ sự không phù hợp nào xảy ra thì việc chuyển đổi là không thể. 
6. Nếu tất cả các bước kiểm tra đều đạt, kết luận rằng việc chuyển đổi là có thể. 

Lý do điều này là đủ vì khi ranh giới khối được cố định, mỗi khối sẽ hoạt động độc lập. Vì chúng ta được phép mở rộng hoặc thu nhỏ các khối một cách tùy ý nên chúng ta luôn có thể khớp độ dài của các khối tương ứng miễn là mẫu trình tự khớp với nhau. 

### Tại sao nó hoạt động 

Điều bất biến là việc phân chia chuỗi nhị phân thành các phân đoạn thống nhất tối đa được xác định duy nhất và không thể thay đổi theo các phép toán được phép. Vì các hoạt động không bao giờ hợp nhất hoặc phá hủy các khối nên mọi phép biến đổi hợp lệ đều phải bảo toàn chuỗi nhận dạng khối chính xác. Do đó, hai chuỗi tương đương nhau theo quy tắc hoạt động khi và chỉ khi chuỗi ký tự được mã hóa theo độ dài chạy của chúng giống hệt nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def rle(s: str):
    blocks = []
    n = len(s)
    i = 0
    while i < n:
        j = i
        while j < n and s[j] == s[i]:
            j += 1
        blocks.append(s[i])
        i = j
    return blocks

def solve():
    t = int(input())
    for _ in range(t):
        a = input().strip()
        b = input().strip()

        ra = rle(a)
        rb = rle(b)

        if ra == rb:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp trực tiếp xây dựng chữ ký độ dài chạy của mỗi chuỗi. Hàm trợ giúp quét từng chuỗi một lần, tạo ra một chuỗi các mã định danh khối. Việc so sánh sau đó được giảm xuống thành một kiểm tra tính bằng nhau trong danh sách đơn giản. 

Một chi tiết triển khai tinh tế là chúng tôi hoàn toàn không lưu trữ độ dài khối. Điều đó là an toàn vì vấn đề rõ ràng cho phép các khối phát triển hoặc thu nhỏ tùy ý. Việc lưu trữ độ dài sẽ chỉ gây ra nhiễu và có nguy cơ so sánh không chính xác. 

Tính chính xác hoàn toàn phụ thuộc vào thực tế là cấu trúc kề là bất biến chứ không phải số lượng thực tế bên trong mỗi khối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét`a = 0001110`Và`b = 001110`. 

Vì`a`, các khối là: 

| Bước | Phạm vi chỉ số | Chặn | 
| --- | --- | --- | 
| 1 | 0-2 | 0 | 
| 2 | 3-5 | 1 | 
| 3 | 6-6 | 0 | 

Vì thế`ra = [0, 1, 0]`. 

Vì`b`, các khối là: 

| Bước | Phạm vi chỉ số | Chặn | 
| --- | --- | --- | 
| 1 | 0-1 | 0 | 
| 2 | 2-4 | 1 | 
| 3 | 5-5 | 0 | 

Vì thế`rb = [0, 1, 0]`. 

Vì các chuỗi khớp nhau nên đầu ra là`YES`. 

Điều này xác nhận rằng độ dài khối khác nhau không thành vấn đề. 

### Ví dụ 2 

hãy để`a = 010`Và`b = 001`. 

Vì`a`, khối là`[0, 1, 0]`. 

Vì`b`, khối là`[0, 1]`. 

| Chuỗi | Chuỗi khối | 
| --- | --- | 
| một | [0, 1, 0] | 
| b | [0, 1] | 

Vì số lượng khối khác nhau nên việc chuyển đổi là không thể, do đó kết quả đầu ra là`NO`. 

Điều này cho thấy rằng ngay cả khi tổng số lượng tương tự nhau, việc luân phiên không khớp sẽ phá vỡ tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi chuỗi được quét một lần để xây dựng cấu trúc độ dài chạy của nó | 
| Không gian | O(n) trường hợp xấu nhất | Trong trường hợp xấu nhất, các chuỗi xen kẽ tạo ra các khối một ký tự | 

Giải pháp này phù hợp thoải mái trong các ràng buộc điển hình cho các vấn đề về chuỗi nhị phân, ngay cả khi tổng kích thước đầu vào đạt tới 10^6 trong tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    def rle(s: str):
        blocks = []
        i = 0
        while i < len(s):
            j = i
            while j < len(s) and s[j] == s[i]:
                j += 1
            blocks.append(s[i])
            i = j
        return blocks

    t = int(sys.stdin.readline())
    for _ in range(t):
        a = sys.stdin.readline().strip()
        b = sys.stdin.readline().strip()
        output.append("YES" if rle(a) == rle(b) else "NO")
    
    return "\n".join(output)

# sample-like tests
assert run("2\n000111\n00111\n010\n001\n") == "YES\nNO"

# minimum size
assert run("1\n0\n0\n") == "YES"

# mismatch start
assert run("1\n1\n0\n") == "NO"

# alternating maximum fragmentation
assert run("1\n010101\n010101\n") == "YES"

# different block structure
assert run("1\n0011\n0101\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ký tự đơn bằng nhau | CÓ | trường hợp hợp lệ tối thiểu | 
| bit bắt đầu khác nhau | KHÔNG | ràng buộc khối bắt đầu | 
| chuỗi xen kẽ giống hệt nhau | CÓ | nhiều khối nhỏ | 
| cùng số lượng nhưng cấu trúc khác nhau | KHÔNG | tầm quan trọng của cấu trúc khối | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi cả hai chuỗi đều bao gồm một ký tự lặp lại. Ví dụ,`a = 0000`Và`b = 0`. Thuật toán nén cả hai thành một khối duy nhất`[0]`, vì vậy đầu ra là`YES`. Điều này phản ánh thực tế là độ dài khối là không liên quan. 

Một trường hợp cạnh khác là khi các chuỗi có số lượng giống nhau`0`Và`1`nhưng các kiểu xen kẽ khác nhau, chẳng hạn như`0011`Và`0101`. Việc đầu tiên sản xuất`[0, 1]`trong khi cái thứ hai tạo ra`[0, 1, 0, 1]`, ngay lập tức bác bỏ phép biến đổi. Thuật toán nắm bắt chính xác điều này vì nó không bao giờ mất thông tin biên trong quá trình nén. 

Trường hợp cạnh cuối cùng là các chuỗi ký tự đơn xen kẽ như`010101`so với`0101010`. Mặc dù cả hai đều trông giống nhau nhưng số lượng khối khác nhau một đơn vị và thuật toán sẽ loại bỏ nó ở bước so sánh, khớp với bất biến rằng cấu trúc khối phải giống hệt nhau.
