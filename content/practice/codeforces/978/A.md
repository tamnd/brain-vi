---
title: "CF 978A - Loại bỏ trùng lặp"
description: "Chúng ta được cung cấp một chuỗi các số nguyên và được yêu cầu nén nó bằng cách loại bỏ các số trùng lặp, nhưng với một quy tắc cụ thể: đối với mỗi giá trị riêng biệt, chỉ phải giữ lại lần xuất hiện cuối cùng của nó trong mảng."
date: "2026-06-17T01:22:28+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 978
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 481 (Div. 3)"
rating: 800
weight: 978
solve_time_s: 82
verified: true
draft: false
---

[CF 978A - Xóa các bản sao](https://codeforces.com/problemset/problem/978/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên và được yêu cầu nén nó bằng cách loại bỏ các số trùng lặp, nhưng với một quy tắc cụ thể: đối với mỗi giá trị riêng biệt, chỉ phải giữ lại lần xuất hiện cuối cùng của nó trong mảng. Đầu ra cuối cùng không chỉ là một tập hợp các giá trị duy nhất mà còn là một phiên bản được lọc của chuỗi ban đầu, trong đó sự xuất hiện trước đó của các số lặp lại sẽ biến mất trong khi sự xuất hiện cuối cùng vẫn giữ nguyên. 

Hạn chế chính là kích thước mảng tối đa là 50. Điều này ngay lập tức loại trừ mọi lo ngại về hiệu suất. Ngay cả một cách tiếp cận kiểm tra từng phần tử với tất cả các phần tử khác cũng sẽ hoạt động tốt trong thời gian giới hạn vì tổng số thao tác tệ nhất sẽ vào khoảng vài nghìn. Điều này có nghĩa là chúng tôi có thể tập trung hoàn toàn vào sự rõ ràng và chính xác thay vì áp lực tối ưu hóa. 

Một vấn đề tế nhị trong các bài toán thuộc loại này là việc “giữ lần xuất hiện cuối cùng” phụ thuộc vào hướng. Một lần quét về phía trước ngây thơ giữ nguyên lần đầu tiên chúng ta nhìn thấy một giá trị sẽ tạo ra câu trả lời sai. Ví dụ, trong mảng`[1, 2, 1]`, giữ nguyên lần xuất hiện đầu tiên mang lại lợi nhuận`[1, 2]`, nhưng đầu ra đúng là`[2, 1]`bởi vì thứ hai`1`là thứ phải ở lại. Một trường hợp lỗi khác phát sinh nếu chúng ta xóa nhanh các phần tử trong khi quét tiến, vì việc dịch chuyển chỉ mục có thể khiến các phần tử sau này bị bỏ qua hoặc xử lý không chính xác. Ví dụ: loại bỏ các bản sao của`5`TRONG`[5, 5, 6]`trong khi lặp lại từ trái sang phải có thể vô tình bỏ qua việc kiểm tra tùy thuộc vào việc triển khai. 

Lý do chính xác phải dựa trên khái niệm “lần xuất hiện ngoài cùng bên phải”, điều này vốn dĩ dễ lý giải hơn khi quét từ cuối thay vì từ đầu. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ xem xét từng phần tử và tìm kiếm ở bên phải để xác định xem nó có xuất hiện lại hay không. Nếu sau này nó không xuất hiện thì chúng tôi giữ lại; nếu không chúng tôi loại bỏ nó. Điều này đúng về mặt logic vì nó trực tiếp mã hóa định nghĩa về “lần xuất hiện cuối cùng”. Tuy nhiên, đối với mỗi`n`các phần tử, chúng tôi có thể quét tới`n`vị trí phía trước, dẫn đến độ phức tạp trong trường hợp xấu nhất là khoảng`n^2`séc. 

Trong khi`n ≤ 50`làm cho điều này trở nên tầm thường trong thực tế, cấu trúc của vấn đề gợi ý một cách tiếp cận tao nhã hơn. Thay vì liên tục tìm kiếm về phía trước, chúng ta có thể đảo ngược quan điểm của mình: xử lý mảng từ phải sang trái trong khi ghi lại những giá trị mà chúng ta đã đưa vào. Lần đầu tiên chúng ta gặp một giá trị trong quá trình duyệt ngược này chính xác là lần xuất hiện cuối cùng của nó trong mảng ban đầu. Quan sát này loại bỏ hoàn toàn nhu cầu quét lặp đi lặp lại. 

Chúng tôi duy trì một tập hợp các giá trị nhìn thấy. Khi chúng ta di chuyển từ cuối mảng lên đầu, chúng ta chỉ lấy một phần tử nếu nó chưa được nhìn thấy trước đó. Điều này đảm bảo rằng mỗi giá trị được thêm chính xác một lần, tại thời điểm tương ứng với lần xuất hiện cuối cùng của nó. Sau khi thu thập kết quả theo thứ tự ngược lại, chúng ta đảo ngược lại để khôi phục lại thứ tự đầu ra đúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét đúng từng phần tử) | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu (quét ngược với bộ) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời bằng cách quét từ cuối mảng để chúng tôi gặp từng giá trị ở lần xuất hiện cuối cùng trước tiên. 

1. Bắt đầu từ phần tử cuối cùng của mảng và di chuyển sang trái về phía phần tử đầu tiên. Hướng này đảm bảo rằng lần đầu tiên chúng ta nhìn thấy một giá trị là lần xuất hiện ngoài cùng bên phải của nó trong chuỗi ban đầu. 
2. Duy trì một bộ lưu trữ các giá trị mà chúng ta đã đưa vào kết quả. Bộ này đại diện cho tất cả các giá trị có lần xuất hiện cuối cùng đã được tính đến. 
3. Đối với mỗi phần tử trong quá trình quét ngược, hãy kiểm tra xem nó đã có trong tập hợp chưa. 

Nếu nó không xuất hiện, điều này có nghĩa là chúng ta hiện đang ở lần xuất hiện cuối cùng của nó, vì vậy chúng ta thêm nó vào kết quả và chèn nó vào tập hợp. 
4. Nếu phần tử đã có trong tập hợp, hãy bỏ qua nó vì lần xuất hiện cuối cùng của nó đã gặp trước đó trong quá trình quét. 
5. Sau khi kết thúc quá trình duyệt ngược, kết quả được thu thập sẽ có thứ tự ngược so với đầu ra mong muốn, vì vậy hãy đảo ngược nó một lần trước khi in. 

### Tại sao nó hoạt động 

Điều bất biến là tại bất kỳ thời điểm nào trong quá trình quét ngược, tập hợp chứa chính xác những giá trị có lần xuất hiện ngoài cùng bên phải trong mảng ban đầu đã được xử lý và lưu trữ. Bởi vì chúng ta duyệt từ phải sang trái, lần đầu tiên chúng ta gặp một giá trị sẽ tương ứng chính xác với vị trí cuối cùng của nó trong mảng ban đầu. Điều này đảm bảo rằng mỗi giá trị được thêm chính xác một lần và không bao giờ được thêm vào trước lần xuất hiện cuối cùng của nó. Đảo ngược trình tự đã thu thập sẽ khôi phục thứ tự tương đối ban đầu trong số những lần xuất hiện cuối cùng đó, vì thứ tự của chúng trong quá trình truyền ngược là đảo ngược thứ tự của chúng trong mảng ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    seen = set()
    res = []
    
    for i in range(n - 1, -1, -1):
        if a[i] not in seen:
            seen.add(a[i])
            res.append(a[i])
    
    res.reverse()
    
    print(len(res))
    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp đọc mảng, sau đó lặp từ phải sang trái. các`seen`set đảm bảo chúng tôi chỉ giữ lại lần đầu tiên mỗi giá trị xuất hiện trong quá trình truyền tải đảo ngược này, tương ứng với lần xuất hiện cuối cùng của nó trong mảng ban đầu. Danh sách kết quả được xây dựng theo thứ tự ngược lại, do đó, việc đảo ngược cuối cùng sẽ khôi phục thứ tự đầu ra chính xác. 

Chi tiết triển khai tinh tế duy nhất là bước đảo ngược là bắt buộc. Nếu không có nó, đầu ra sẽ liệt kê các phần tử theo thứ tự lần xuất hiện cuối cùng của chúng từ phải sang trái, đây không phải là định dạng bắt buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 5 5 1 6 1
```Chúng tôi quét từ phải sang trái. 

| Chỉ mục | Giá trị | Đã thấy trước đây? | Hành động | Kết quả cho đến nay | 
| --- | --- | --- | --- | --- | 
| 5 | 1 | không | thêm 1 | [1] | 
| 4 | 6 | không | thêm 6 | [1, 6] | 
| 3 | 1 | vâng | bỏ qua | [1, 6] | 
| 2 | 5 | không | thêm 5 | [1, 6, 5] | 
| 1 | 5 | vâng | bỏ qua | [1, 6, 5] | 
| 0 | 1 | vâng | bỏ qua | [1, 6, 5] | 

Sau khi đảo chiều:```
5 6 1
```Điều này cho thấy rằng mỗi giá trị được ghi lại chính xác ở vị trí cuối cùng của nó và các giá trị trùng lặp trước đó sẽ bị bỏ qua. 

### Ví dụ 2 

đầu vào:```
5
2 3 2 4 3
```| Chỉ mục | Giá trị | Đã thấy trước đây? | Hành động | Kết quả cho đến nay | 
| --- | --- | --- | --- | --- | 
| 4 | 3 | không | thêm 3 | [3] | 
| 3 | 4 | không | thêm 4 | [3, 4] | 
| 2 | 2 | không | thêm 2 | [3, 4, 2] | 
| 1 | 3 | vâng | bỏ qua | [3, 4, 2] | 
| 0 | 2 | vâng | bỏ qua | [3, 4, 2] | 

Sau khi đảo chiều:```
2 4 3
```Dấu vết này xác nhận rằng thuật toán duy trì thứ tự của các lần xuất hiện cuối cùng trong khi loại bỏ các bản sao trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần trong quá trình truyền tải ngược, với các phép toán tập hợp trung bình O(1) | 
| Không gian | O(n) | Danh sách tập hợp và kết quả cùng nhau lưu trữ tối đa n phần tử | 

Cho rằng`n ≤ 50`, điều này nằm trong giới hạn, nhưng cách tiếp cận tuyến tính vẫn là giải pháp rõ ràng và tổng quát nhất. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        
        seen = set()
        res = []
        
        for i in range(n - 1, -1, -1):
            if a[i] not in seen:
                seen.add(a[i])
                res.append(a[i])
        
        res.reverse()
        print(len(res))
        print(*res)
    
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("6\n1 5 5 1 6 1\n") == "3\n5 6 1"

# custom cases
assert run("1\n7\n") == "1\n7"
assert run("4\n1 1 1 1\n") == "1\n1"
assert run("5\n1 2 3 4 5\n") == "5\n1 2 3 4 5"
assert run("5\n5 4 3 2 1\n") == "5\n5 4 3 2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | chính nó | xử lý đầu vào tối thiểu | 
| tất cả đều bình đẳng | một phần tử | sụp đổ trùng lặp | 
| đã tăng độc đáo | không thay đổi | không cần xóa | 
| thứ tự ngược lại | giữ gìn trật tự | đặt hàng ổn định | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[7]`, quét ngược lại ngay lập tức thêm`7`đến kết quả vì nó không được nhìn thấy. Không còn phần tử nào tồn tại nữa nên kết quả vẫn giữ nguyên`[7]`, phù hợp với yêu cầu rằng một giá trị duy nhất là lần xuất hiện cuối cùng của chính nó. 

Đối với một mảng trong đó tất cả các phần tử đều giống hệt nhau, chẳng hạn như`[1, 1, 1, 1]`, quá trình quét ngược cho biết thêm`1`ở vị trí cuối cùng và sau đó bỏ qua tất cả các lần xuất hiện trước đó vì chúng đã ở vị trí`seen`bộ. Kết quả chứa chính xác một phần tử, điều này đúng vì chỉ còn lại lần xuất hiện ngoài cùng bên phải. 

Đối với một mảng tăng nghiêm ngặt như`[1, 2, 3, 4]`, mọi phần tử lần đầu tiên được gặp trong quá trình quét ngược ở lần xuất hiện duy nhất của nó, vì vậy tất cả đều được bao gồm. Việc đảo ngược khôi phục lại thứ tự ban đầu không thay đổi, xác nhận rằng thuật toán không loại bỏ các phần tử một cách không cần thiết.
