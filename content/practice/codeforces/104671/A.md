---
title: "CF 104671A - Tối đa hóa chất lượng bữa ăn"
description: "Chúng ta được cung cấp một tập hợp các con số đại diện cho chất lượng của thành phần. Chúng ta phải chia các số này thành đúng k nhóm không trống, trong đó mỗi số thuộc đúng một nhóm. Mỗi nhóm đại diện cho một món ăn. Điểm của một món ăn được xác định theo một cách hơi khác thường."
date: "2026-06-29T09:27:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104671
codeforces_index: "A"
codeforces_contest_name: "2023 ICPC Columbia University Local Contest"
rating: 0
weight: 104671
solve_time_s: 61
verified: true
draft: false
---

[CF 104671A - Tối đa hóa chất lượng bữa ăn](https://codeforces.com/problemset/problem/104671/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các con số đại diện cho chất lượng của thành phần. Chúng ta phải chia những con số này thành chính xác`k`các nhóm không trống, trong đó mỗi số thuộc đúng một nhóm. Mỗi nhóm đại diện cho một món ăn. 

Điểm của một món ăn được xác định theo một cách hơi khác thường. Nếu một món ăn có chứa các giá trị thì điểm của nó là tổng của tất cả các giá trị trong đó cộng với giá trị tối đa trong món ăn đó một lần nữa. Vì vậy, nếu một nhóm có các phần tử thì phần tử lớn nhất đóng góp hai lần trong khi tất cả các phần tử khác đóng góp một lần. 

Mục tiêu là chọn cách phân vùng mảng thành`k`nhóm sao cho tổng điểm của tất cả các món ăn càng lớn càng tốt. 

Khó khăn chính là các quyết định phân nhóm ảnh hưởng đến việc liệu một giá trị có trở thành giá trị tối đa trong nhóm của nó hay chỉ là một thành phần đóng góp thường xuyên. Vì mọi nhóm đều cộng lại mức tối đa của nó nên cấu trúc của các nhóm quan trọng hơn chỉ là tổng các giá trị. 

Các ràng buộc cho phép lên đến`2 * 10^5`các phần tử, vì vậy mọi giải pháp đều phải gần với tuyến tính hoặc tuyến tính. Chiến lược nhóm kiểu O(n^2) hoặc thậm chí O(nk) ngay lập tức quá chậm vì nó có thể yêu cầu lặp lại nhiều phân vùng có thể có hoặc theo dõi quá trình chuyển đổi giữa số lượng nhóm và bài tập. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị giống hệt nhau. Ví dụ, nếu tất cả`a_i = 1`Và`k = n`, mọi phần tử phải ở một mình và câu trả lời sẽ trở thành`2n`. Một trực giác ngây thơ có thể cho rằng việc phân nhóm không quan trọng lắm trong những trường hợp thống nhất như vậy, nhưng ngay cả khi đó “mức tối đa bổ sung cho mỗi nhóm” vẫn buộc mỗi nhóm đơn lẻ phải đóng góp một giá trị bổ sung. 

Một trường hợp cạnh quan trọng khác là khi`k = 1`. Khi đó tất cả các phần tử đều thuộc một nhóm và câu trả lời đơn giản là`sum(a) + max(a)`. Bất kỳ thuật toán nào giả định việc chia tách luôn có lợi sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi phân vùng có thể có của mảng thành`k`các nhóm. Đối với mỗi phân vùng, nó sẽ tính tổng của từng nhóm và mức tối đa của nó, sau đó tính tổng giữa các nhóm. Số lượng các phân vùng tăng lên một cách tổ hợp, về cơ bản bị chi phối bởi số Stirling loại hai. Ngay cả việc hạn chế các phân vùng liền kề cũng không giúp ích được gì, vì vấn đề không yêu cầu các nhóm phải liền kề nhau. 

Ngay cả phương pháp lập trình động theo dõi số lượng phần tử đã được gán cho bao nhiêu nhóm cũng nhanh chóng trở nên không khả thi vì chúng ta cần biết, đối với mỗi tiền tố và số lượng nhóm, cách phân phối cực đại một cách tối ưu. Không gian trạng thái trở nên quá lớn đối với`n`lên đến`2e5`. 

Cái nhìn sâu sắc quan trọng là diễn giải lại những gì tạo ra giá trị trong một nhóm. Mỗi phần tử đóng góp chính xác một lần vào tổng số thông qua tổng của tất cả các nhóm. Phần đóng góp bổ sung duy nhất đến từ việc mỗi nhóm cộng thêm tối đa một lần nữa. 

Vì vậy, toàn bộ tối ưu hóa giảm xuống mức tối đa hóa tổng cực đại của nhóm đã chọn. Vì chúng ta có chính xác`k`nhóm, chúng ta cần chọn`k`các yếu tố đóng vai trò là “người lãnh đạo nhóm” đóng vai trò tối đa cho các nhóm tương ứng của họ. Mọi phần tử khác sẽ chỉ đóng góp một lần thông qua tổng toàn cầu. 

Điều này có nghĩa là chúng ta nên tối đa hóa tổng`k`phần tử được chọn đóng vai trò cực đại. Đương nhiên, chúng tôi muốn`k`các phần tử lớn nhất trong mảng, vì chúng mang lại sự đóng góp bổ sung lớn nhất có thể. Phần còn lại`n-k`các phần tử sẽ chỉ là thành viên thường xuyên đóng góp một lần. 

Như vậy, đáp án cuối cùng sẽ là tổng của tất cả các phần tử cộng với tổng của`k`phần tử lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Siêu lũy thừa | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) đến O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng các phần tử trong mảng. Điều này thể hiện sự đóng góp cơ bản trong đó mỗi phần tử được tính chính xác một lần bất kể cấu trúc nhóm. 
2. Sắp xếp mảng theo thứ tự giảm dần. Điều này cho phép chúng ta dễ dàng xác định các phần tử sẽ đóng vai trò là ứng cử viên cực đại của nhóm. 
3. Lấy cái đầu tiên`k`các phần tử theo thứ tự sắp xếp này. Chúng đại diện cho những lựa chọn tốt nhất có thể có cho các phần tử sẽ đóng góp thêm thời gian dưới dạng cực đại của nhóm. 
4. Cộng tổng của chúng`k`các phần tử thành tổng cơ sở. Điều này chiếm phần đóng góp bổ sung của phần tử tối đa của mỗi nhóm. 
5. Xuất ra giá trị kết quả dưới dạng tổng điểm tối đa có thể đạt được. 

Lý do chúng tôi đặc biệt lấy lớn nhất`k`các phần tử là mỗi nhóm đóng góp chính xác một phần tử tối đa và không có hạn chế nào ngăn cản việc chỉ định bất kỳ phần tử đã chọn nào làm phần tử tối đa của một nhóm. Vì các nhóm là tùy ý nên chúng ta luôn có thể đặt mỗi mức tối đa đã chọn vào nhóm riêng của nó và phân phối các phần tử còn lại một cách tự do. 

### Tại sao nó hoạt động 

Mỗi phần tử đóng góp ít nhất một lần nên tổng cơ sở là cố định. Sự tự do duy nhất nằm ở việc lựa chọn phần tử nào trở thành cực đại. Mỗi nhóm thêm chính xác một khoản đóng góp bổ sung bằng mức tối đa của nó, vì vậy chúng tôi đang lựa chọn một cách hiệu quả`k`các yếu tố để nhận được tiền thưởng bằng giá trị của chúng. Tối đa hóa phần thưởng này một cách độc lập dẫn trực tiếp đến việc chọn mức lớn nhất`k`các giá trị, vì không có ràng buộc ghép nối giữa các nhóm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    total = sum(a)
    a.sort(reverse=True)
    
    bonus = sum(a[:k])
    print(total + bonus)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính tổng của tất cả các thành phần, đây là sự đóng góp cơ bản không thể tránh khỏi. Sau đó, nó sắp xếp mảng theo thứ tự giảm dần để có thể truy cập được các phần tử lớn nhất theo thứ tự tuyến tính. 

Bước quan trọng là chọn đầu`k`các yếu tố như là nguồn đóng góp thêm. Vì mỗi nhóm đóng góp chính xác một mức tối đa nên chúng ta có thể coi điều này giống như chỉ định một “vị trí thưởng” cho mỗi nhóm và lấp đầy các vị trí đó bằng các giá trị sẵn có lớn nhất sẽ tối đa hóa kết quả. 

Các yếu tố còn lại không ảnh hưởng đến thời hạn thưởng và chỉ đóng góp thông qua tổng cơ sở nên có thể bỏ qua sau khi sắp xếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 3
7 3 9 1 2 7
```Mảng được sắp xếp là`[9, 7, 7, 3, 2, 1]`. 

| Bước | Hành động | Giá trị | 
| --- | --- | --- | 
| 1 | Tổng số tiền | 29 | 
| 2 | Yếu tố k hàng đầu | [9, 7, 7] | 
| 3 | Số tiền thưởng | 23 | 
| 4 | Câu trả lời cuối cùng | 52 | 

Điều này khớp với một phân vùng trong đó ba nhóm, mỗi nhóm “yêu cầu” một trong những phần tử lớn nhất làm mức tối đa, tối đa hóa phần đóng góp bổ sung. 

### Mẫu 2 

đầu vào:```
5 5
1 1 1 1 1
```Mảng được sắp xếp là`[1, 1, 1, 1, 1]`. 

| Bước | Hành động | Giá trị | 
| --- | --- | --- | 
| 1 | Tổng số tiền | 5 | 
| 2 | Yếu tố k hàng đầu | [1, 1, 1, 1, 1] | 
| 3 | Số tiền thưởng | 5 | 
| 4 | Câu trả lời cuối cùng | 10 | 

Mỗi phần tử tạo thành nhóm riêng và mỗi nhóm đóng góp phần tử đơn lẻ của nó hai lần. 

Điều này xác nhận rằng ngay cả khi việc nhóm buộc phải tầm thường thì công thức vẫn đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế sau khi quét tuyến tính để tính tổng | 
| Không gian | O(1) thêm (hoặc O(n)) | Tùy thuộc vào việc triển khai sắp xếp tại chỗ | 

Các ràng buộc cho phép lên đến`2 * 10^5`các phần tử và một`n log n`giải pháp thoải mái phù hợp trong thời hạn. Việc sử dụng bộ nhớ là tuyến tính ở kích thước đầu vào và duy trì trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    total = sum(a)
    a.sort(reverse=True)
    bonus = sum(a[:k])
    return str(total + bonus)

# provided samples
assert run("6 3\n7 3 9 1 2 7\n") == "52"
assert run("5 5\n1 1 1 1 1\n") == "10"

# custom cases
assert run("1 1\n10\n") == "20", "single element"
assert run("4 1\n5 1 2 3\n") == str(sum([5,1,2,3]) + 5), "single group"
assert run("6 2\n1 100 1 1 100 1\n") == str(sum([1,100,1,1,100,1]) + 200), "two large maxima"
assert run("3 3\n2 2 2\n") == "12", "all equal split"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 20 | k = n = 1 ranh giới | 
| nhóm đơn | tổng + tối đa | k = 1 hành vi | 
| hai cực đại lớn | lựa chọn giá trị lớn | sự lựa chọn tham lam đúng đắn | 
| chia đều bằng nhau | 2n | trường hợp cạnh thống nhất | 

## Vỏ cạnh 

Khi nào`k = n`, mọi phần tử phải tạo thành nhóm riêng của nó. Thuật toán chọn tất cả các phần tử làm đầu`k`, do đó tiền thưởng trở thành tổng số tiền đầy đủ, tạo ra`2 * sum(a)`, điều này phù hợp với thực tế là mỗi nhóm đơn lẻ đóng góp phần tử của nó hai lần. 

Khi`k = 1`, thuật toán chỉ chọn phần tử lớn nhất làm phần thưởng, đưa ra`sum(a) + max(a)`. Điều này tương ứng với việc đặt mọi thứ vào một nhóm, trong đó chỉ có một mức tối đa được thêm vào. 

Khi tất cả các giá trị đều bằng nhau, hãy nói`[x, x, x]`với`k = 2`, thuật toán vẫn chọn hai phần tử bất kỳ làm người đóng góp tiền thưởng, tạo ra`3x + 2x = 5x`. Bất kỳ phân vùng nào cũng mang lại cấu trúc giống nhau vì mỗi nhóm tối đa đều giống hệt nhau, xác nhận tính đúng đắn của việc chọn các phần tử trên cùng tùy ý trong các tình huống ràng buộc.
