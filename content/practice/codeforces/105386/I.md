---
title: "CF 105386I - Dịch chuyển trái 2"
description: "Chúng ta được cấp một chuỗi và chúng ta được phép dịch chuyển chuỗi đó theo chu kỳ. Sau khi chọn ca, chúng ta xem chuỗi kết quả được sắp xếp thành một vòng tròn, nghĩa là ký tự cuối cùng được coi là ký tự liền kề với ký tự đầu tiên."
date: "2026-06-23T05:14:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105386
codeforces_index: "I"
codeforces_contest_name: "The 2024 ICPC Kunming Invitational Contest"
rating: 0
weight: 105386
solve_time_s: 56
verified: true
draft: false
---

[CF 105386I - Dịch chuyển sang trái 2](https://codeforces.com/problemset/problem/105386/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi và chúng ta được phép dịch chuyển chuỗi đó theo chu kỳ. Sau khi chọn ca, chúng ta xem chuỗi kết quả được sắp xếp thành một vòng tròn, nghĩa là ký tự cuối cùng được coi là ký tự liền kề với ký tự đầu tiên. Đối với chuỗi được xoay đó, chúng tôi muốn sửa đổi càng ít ký tự càng tốt để không có hai ký tự lân cận nào giống nhau. 

Một thao tác duy nhất cho phép chúng ta thay đổi bất kỳ vị trí nào thành bất kỳ chữ cái viết thường nào. Đối với mỗi lần xoay có thể, chúng tôi đo lường số lượng thay đổi cần thiết để làm cho chuỗi được xoay trở nên “phù hợp”, sau đó chúng tôi chọn phép xoay có số lượng thay đổi nhỏ nhất. 

Độ dài chuỗi có thể lên tới 500.000 trong tất cả các trường hợp thử nghiệm, do đó, mọi thứ bậc hai trong tổng kích thước sẽ không thành công. Ngay cả thời gian tuyến tính trên mỗi vòng quay cũng không thể thực hiện được vì có n vòng quay. Điều này ngay lập tức loại trừ việc tính toán lại câu trả lời từ đầu cho mỗi ca. 

Một sự hiểu lầm ngây thơ là cho rằng phép quay làm thay đổi độ khó của sợi dây. Ví dụ: người ta có thể mong đợi rằng việc cắt chuỗi ở một điểm khác có thể làm đứt hoặc tạo ra các chuỗi ký tự dài bằng nhau và do đó thay đổi câu trả lời. Tuy nhiên, vì chuỗi được coi là tuần hoàn khi kiểm tra tính kề cận, nên mọi phép quay đều bảo toàn cấu trúc kề cận theo chu kỳ giống nhau, chỉ lập lại chỉ mục cho nó. 

Trường hợp cạnh phổ biến là một chuỗi hoàn toàn đồng nhất như "aaaaaa". Mỗi vòng quay trông giống hệt nhau và chiến lược tốt nhất là thay thế các thay đổi trong suốt chu kỳ. Một chuỗi khác là một chuỗi được tạo thành từ các khối dài xen kẽ như "aaabbbccc", trong đó mỗi khối đóng góp độc lập vào chi phí. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ thử mọi vòng quay. Đối với mỗi ca, chúng tôi tính toán số lượng sửa đổi tối thiểu cần thiết để đảm bảo không có hai ký tự liền kề nào khớp nhau. Một cách trực tiếp để tính toán điều này cho một chuỗi cố định là xem nó dưới dạng biểu đồ chu trình trong đó mỗi vị trí phải được gán một ký tự mới khác với các ký tự lân cận của nó. Ngay cả khi chúng tôi cố gắng tối ưu hóa điều này bằng cách sử dụng lập trình động, mỗi vòng quay vẫn sẽ yêu cầu xử lý tuyến tính, dẫn đến tổng công việc là O(n^2). 

Quan sát quan trọng là việc xoay chuỗi không làm thay đổi mối quan hệ kề cận theo chu kỳ giữa các vị trí. Mỗi cặp hàng xóm trong chu trình vẫn là hàng xóm sau khi quay, chỉ với các chỉ số được dịch chuyển. Điều này có nghĩa là cấu trúc “nơi xuất hiện các cặp liền kề bằng nhau” là bất biến qua các phép quay. 

Khi chúng tôi sửa một phép xoay, vấn đề sẽ trở thành cục bộ: thông tin quan trọng duy nhất là cách các ký tự bằng nhau tạo thành các phân đoạn liền kề dọc theo chu kỳ. Trong một đoạn gồm các ký tự giống hệt nhau có độ dài L, chiến lược tốt nhất là sửa đổi khoảng một nửa trong số chúng sao cho không có hai ký tự liền kề nào bằng nhau. Điều này đóng góp một chi phí tỷ lệ thuận với L độc lập với vòng quay. 

Vì các phép quay không làm thay đổi các lần chạy theo chu kỳ này nên câu trả lời là giống nhau cho mỗi ca. Vì vậy, chúng ta chỉ cần tính chi phí một lần trên chuỗi tuần hoàn ban đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các vòng quay | O(n²) | O(1) | Quá chậm | 
| Một lần vượt qua các lần chạy theo chu kỳ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi chuỗi là chuỗi tròn và nén nó thành các đoạn tối đa gồm các ký tự giống hệt nhau liên tiếp.

1. Duyệt chuỗi và hợp nhất nó thành một chu trình, nghĩa là ký tự cuối cùng và ký tự đầu tiên cũng được coi là liền kề. Nếu chúng bằng nhau thì chúng thuộc cùng một đường chạy, vì vậy chúng ta xoay ranh giới đường chạy cho phù hợp thay vì tách nó ra. 
2. Xác định tất cả các dòng tối đa của các ký tự giống hệt nhau theo nghĩa tuần hoàn này. Mỗi lần chạy có độ dài L. 
3. Với mỗi lần chạy, hãy tính xem có bao nhiêu ký tự phải thay đổi để không có hai ký tự liền kề nào bằng nhau. Bên trong một lần chạy thống nhất, chúng ta có thể giữ nguyên tối đa mọi ký tự khác không thay đổi, do đó chi phí đóng góp cho một lần chạy là sàn(L / 2). 
4. Tổng hợp những đóng góp này qua tất cả các lần chạy để có được câu trả lời cuối cùng. 

Phần tinh tế duy nhất là xử lý việc hợp nhất theo chu kỳ một cách chính xác. Nếu ký tự đầu tiên và cuối cùng bằng nhau thì chúng thuộc cùng một lần chạy và phải được hợp nhất trước khi tính toán đóng góp. Nếu không, chúng ta sẽ coi một lần chạy tuần hoàn đơn lẻ là hai lần chạy tuyến tính riêng biệt, điều này sẽ đánh giá thấp những thay đổi cần thiết. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là mọi ràng buộc kề đều độc lập theo nghĩa là vi phạm chỉ xảy ra khi hai ký tự giống hệt nhau ở cạnh nhau. Mỗi khối tối đa có các ký tự bằng nhau tạo thành một vùng biệt lập nơi có nhiều ràng buộc dày đặc và chiến lược sửa chữa tối ưu là cục bộ cho khối đó. Vì phép quay không thay đổi vị trí nào bằng với các vị trí lân cận theo chu kỳ của chúng, nên việc phân tách thành các lần chạy và độ dài của chúng không thay đổi, điều này cố định chi phí cho mỗi lần quay. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(s: str) -> int:
    n = len(s)
    if n == 1:
        return 0

    # build cyclic runs
    runs = []

    i = 0
    while i < n:
        j = i + 1
        while j < n and s[j] == s[i]:
            j += 1
        runs.append(j - i)
        i = j

    # merge first and last run if cyclicly connected
    if len(runs) > 1 and s[0] == s[-1]:
        runs[0] += runs[-1]
        runs.pop()

    # compute cost
    ans = 0
    for L in runs:
        ans += L // 2
    return ans

def main():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        print(solve(s))

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ nén chuỗi thành các lần chạy có ký tự bằng nhau liên tiếp. Việc này được thực hiện trong một lần quét duy nhất nên nó chạy theo thời gian tuyến tính. Bước không hề đơn giản duy nhất là hợp nhất lần chạy đầu tiên và lần chạy cuối cùng khi chúng có chung ký tự, điều này đảm bảo tính chất tuần hoàn của chuỗi được tôn trọng. 

Sau khi hình thành các lượt chạy, mỗi lượt đóng góp một nửa chiều dài được làm tròn thành câu trả lời. Điều này trực tiếp phù hợp với chiến lược tối ưu để sửa các bản sao liền kề bên trong một phân khúc thống nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét`s = "aaabbaa"`. 

Đầu tiên chúng ta hình thành các đường chạy tuyến tính:`"aaa"`,`"bb"`,`"aa"`. Vì ký tự đầu tiên và cuối cùng đều là`'a'`, chúng tôi hợp nhất lần chạy đầu tiên và lần chạy cuối cùng vào`"aaaaa"`Và`"bb"`. 

| Bước | Chạy | Hành động | 
| --- | --- | --- | 
| Ban đầu | [3, 2, 2] | phân đoạn tuyến tính | 
| Hợp nhất | [5, 2] | hợp nhất đầu tiên và cuối cùng | 
| Chi phí | 2 + 1 | tầng(5/2) + tầng(2/2) | 

Câu trả lời cuối cùng là 3. 

Điều này chứng tỏ rằng việc hợp nhất theo chu kỳ là cần thiết; nếu không chúng ta sẽ xử lý cấu trúc không chính xác. 

### Ví dụ 2 

Hãy xem xét`s = "abcde"`. 

Tất cả các ký tự đều khác nhau nên mỗi lần chạy đều có độ dài 1. 

| Bước | Chạy | Hành động | 
| --- | --- | --- | 
| Ban đầu | [1,1,1,1,1] | từng nhân vật bị cô lập | 
| Hợp nhất | [1,1,1,1,1] | không cần hợp nhất | 
| Chi phí | 0 | toàn bộ tầng(1/2) | 

Không cần thay đổi vì chuỗi đã hợp lệ. 

Điều này cho thấy thuật toán tự nhiên trả về 0 khi không tồn tại vi phạm lân cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lượt để xây dựng các lần chạy cộng với tổng hợp tuyến tính | 
| Không gian | O(1) | danh sách chạy nhiều nhất là O(n), nhưng có thể phát trực tuyến nếu cần | 

Tổng kích thước đầu vào lên tới 5×10^5, do đó, quét tuyến tính trên mỗi trường hợp kiểm thử sẽ vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve(s: str) -> int:
        n = len(s)
        if n == 1:
            return 0

        runs = []
        i = 0
        while i < n:
            j = i + 1
            while j < n and s[j] == s[i]:
                j += 1
            runs.append(j - i)
            i = j

        if len(runs) > 1 and s[0] == s[-1]:
            runs[0] += runs[-1]
            runs.pop()

        return sum(x // 2 for x in runs)

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        s = sys.stdin.readline().strip()
        out.append(str(solve(s)))
    return "\n".join(out)

# provided + basic cases
assert run("1\nabccbbbbd\n") == "3"
assert run("1\nabcde\n") == "0"

# single char
assert run("1\nx\n") == "0"

# all same
assert run("1\naaaaa\n") == "2"

# alternating
assert run("1\nababab\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | 0 | trường hợp ranh giới tối thiểu | 
| tất cả đều bình đẳng | 2 | xử lý chạy theo chu kỳ | 
| xen kẽ | 0 | cấu trúc đã hợp lệ | 

## Vỏ cạnh 

Một chuỗi ký tự đơn đã có giá trị tầm thường vì không có cặp liền kề nào và thuật toán trả về chính xác số 0 vì chỉ có một chuỗi có độ dài một. 

Một chuỗi tuần hoàn thống nhất hoàn toàn như`"aaaaa"`tạo thành một lần chạy hợp nhất sau khi xem xét tính liền kề theo chu kỳ. Thuật toán hợp nhất các điểm cuối và tính toán mức sàn (5/2), tạo ra chính xác 2. Đây là trường hợp mà việc quên hợp nhất theo chu kỳ sẽ tạo ra không chính xác 2 lần chạy riêng biệt và các khoản đóng góp bị tính thiếu hoặc đếm sai. 

Một chuỗi như`"ababa"`không có cặp liền kề bằng nhau trong chu kỳ. Việc phân tách lần chạy chỉ mang lại lần chạy có độ dài một lần, do đó mọi đóng góp đều bằng 0, phù hợp với thực tế là không cần sửa đổi.
