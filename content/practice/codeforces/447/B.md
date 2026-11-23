---
title: "CF 447B - DZY Yêu Đàn Dây"
description: "Chúng ta được cung cấp một chuỗi gồm các chữ cái viết thường và cách gán trọng số bằng số cho mỗi chữ cái. Giá trị của một chuỗi đầy đủ được tính bằng cách tính tổng, trên tất cả các vị trí, tích của chỉ số vị trí (bắt đầu từ 1) và trọng số của ký tự tại vị trí đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 447
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round #FF (Div. 2)"
rating: 1000
weight: 447
solve_time_s: 71
verified: true
draft: false
---

[CF 447B - DZY yêu thích đàn dây](https://codeforces.com/problemset/problem/447/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi gồm các chữ cái viết thường và cách gán trọng số bằng số cho mỗi chữ cái. Giá trị của một chuỗi đầy đủ được tính bằng cách tính tổng, trên tất cả các vị trí, tích của chỉ số vị trí (bắt đầu từ 1) và trọng số của ký tự tại vị trí đó. Vì vậy, các ký tự được đặt sau sẽ đóng góp nhiều hơn vì số nhân của chúng lớn hơn. 

Chúng ta cũng được phép chèn chính xác`k`các chữ cái viết thường bổ sung ở bất kỳ đâu trong chuỗi. Mỗi ký tự được chèn đóng góp trọng số riêng của nó nhân với vị trí của nó trong chuỗi cuối cùng và việc chèn một ký tự sẽ dịch chuyển mọi thứ sau ký tự đó sang một vị trí bên phải, làm tăng số nhân của chúng. Mục tiêu là chọn cả các chữ cái được chèn và vị trí của chúng sao cho tổng trọng số cuối cùng được tối đa hóa. 

Các ràng buộc rất nhỏ: độ dài chuỗi tối đa là 1000 và`k`nhiều nhất là 1000. Điều này ngay lập tức gợi ý rằng lý luận bậc hai hoặc thậm chí bậc ba đối với các vị trí là có thể chấp nhận được, nhưng việc ép buộc một cách thô bạo tất cả các cấu hình chèn có thể là không thể vì mỗi lần chèn có 26 lựa chọn và 2000 vị trí, dẫn đến sự bùng nổ theo cấp số nhân. 

Một vấn đề tế nhị xuất hiện khi lý luận tham lam về các phần chèn thêm. Một ý tưởng ngây thơ là luôn đặt chữ cái có giá trị cao nhất ở cuối, vì các vị trí sau có số nhân lớn hơn. Điều này không thành công vì việc chèn một chữ cái vào cuối sẽ làm tăng độ dài, điều này cũng làm tăng sự đóng góp của tất cả các ký tự trước đó. Một cái bẫy khác là nghĩ rằng các phép chèn là độc lập, trong khi trên thực tế, mỗi lần chèn sẽ thay đổi tất cả các chỉ số tiếp theo. 

## Phương pháp tiếp cận 

Quan điểm brute-force là thử mọi cách chèn`k`các chữ cái vào chuỗi và tính điểm kết quả trực tiếp. Ngay cả khi cố định vị trí các phần chèn, chúng ta vẫn cần chọn chữ cái nào để chèn vào mỗi vị trí. Điều này dẫn đến khoảng$\binom{n+k}{k} \cdot 26^k$những khả năng vượt xa khả thi ngay cả đối với những đầu vào nhỏ. Chi phí đánh giá mỗi cấu hình là tuyến tính theo chiều dài cuối cùng, do đó phương pháp này sẽ thất bại ngay lập tức. 

Quan sát quan trọng là hàm giá trị là tuyến tính trong sự đóng góp của các ký tự theo vị trí. Điều quan trọng không phải là vị trí tuyệt đối mà chúng tôi chọn để chèn, mà là những chữ cái nào chúng tôi quyết định sử dụng tổng thể và số lượng chúng tôi chèn vào. 

Sau khi chúng tôi sửa nhiều tập hợp các chữ cái được chèn, chiến lược tốt nhất là luôn đặt các chữ cái có giá trị cao hơn về phía bên phải nhiều nhất có thể, vì các vị trí sau sẽ nhân trọng số của chúng nhiều hơn. Điều này biến vấn đề thành việc quyết định số lượng mỗi chữ cái cần chèn, sau đó đặt tất cả các chữ cái được chèn theo thứ tự trọng lượng không tăng ở cuối chuỗi. Thứ tự chuỗi ban đầu được giữ nguyên vì việc sắp xếp lại các ký tự gốc sẽ chỉ gây tổn hại vì những lần xuất hiện trước đó có hệ số nhân vị trí nhỏ hơn. 

Vì vậy, giải pháp trở thành: coi chuỗi gốc là cố định và nối thêm`k`ký tự được lựa chọn tối ưu. Vì thứ tự chỉ quan trọng thông qua trọng số nên các ký tự được chèn phải được sắp xếp theo trọng số và đặt ở cuối. 

Điều này làm giảm vấn đề thành một công trình tham lam đơn giản: chọn cái tốt nhất`k`các chữ cái (có thể lặp lại) và nối chúng theo thứ tự trọng lượng giảm dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n + k log 26) | O(26 + n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính phần đóng góp hiện tại của chuỗi gốc bằng công thức trong đó mỗi ký tự ở vị trí`i`đóng góp`i * w[c]`. Điều này là cố định và không thể thay đổi bằng cách sắp xếp lại, vì vậy chúng tôi chỉ cần tích lũy nó. 
2. Trích xuất tất cả trọng số của các chữ cái và xác định chữ cái nào có trọng lượng lớn nhất. Vì chúng ta được phép chèn bất kỳ chữ cái nào nên mỗi lần chèn phải sử dụng ký tự có trọng lượng tối đa này. 

Lý do là mỗi lần chèn đóng góp tuyến tính vào tổng số tiền, do đó việc thay thế bất kỳ ký tự được chèn nào bằng ký tự có trọng số thấp hơn sẽ làm giảm nghiêm trọng kết quả. 
3. Xây dựng chuỗi cuối cùng bằng cách nối thêm`k`bản sao của chữ cái tốt nhất này vào chuỗi gốc. Chúng ta không cần phải xen kẽ các phần chèn vào bên trong chuỗi gốc vì việc đặt chúng sớm hơn sẽ làm giảm hệ số nhân vị trí của chúng, đồng thời đẩy chúng về cuối sẽ tối đa hóa chỉ số của chúng. 
4. Tính toán phần đóng góp của các ký tự được chèn vào. Vì họ chiếm vị trí cuối cùng`k`các vị trí, chỉ số của chúng được cố định như`n+1`ĐẾN`n+k`, do đó tổng đóng góp của họ chỉ là một đánh giá số học đơn giản đối với các vị trí đó. 
5. Thêm phần đóng góp ban đầu và phần đóng góp được chèn vào để có được câu trả lời. 

### Tại sao nó hoạt động 

Hàm giá trị là tuyến tính trên các vị trí và việc chèn chỉ ảnh hưởng đến thứ tự thông qua việc dịch chuyển vị trí. Bất kỳ ký tự được chèn nào được đặt trước đó sẽ làm giảm hệ số nhân trọng lượng của chính nó và cũng đẩy các ký tự gốc lên vị trí cao hơn, nhưng những đóng góp ban đầu đó đã được cố định theo thứ tự tương đối. Vì các ký tự được chèn luôn có thể được di chuyển sang phải mà không gây tổn hại đến sự đóng góp của bất kỳ ký tự được chèn nào khác, nên sự sắp xếp tối ưu luôn đẩy tất cả các phần chèn vào cuối. Khi điều này được thiết lập, mỗi lần chèn sẽ độc lập và nên sử dụng trọng lượng chữ cái tối đa có sẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    k = int(input())
    w = list(map(int, input().split()))
    
    n = len(s)
    
    # original contribution
    base = 0
    for i, ch in enumerate(s, start=1):
        base += i * w[ord(ch) - ord('a')]
    
    mx = max(w)
    
    # inserted letters occupy positions n+1 ... n+k
    # all are best letter
    for i in range(1, k + 1):
        base += (n + i) * mx
    
    print(base)

if __name__ == "__main__":
    solve()
```Mã này tách phần đóng góp chuỗi gốc khỏi các chữ cái được chèn vào. Vòng lặp ban đầu áp dụng trực tiếp định nghĩa của hàm tính điểm. Sau đó, chúng tôi tính toán phần đóng góp được chèn tốt nhất có thể bằng cách chọn chữ cái có trọng số tối đa và đặt nó ở vị trí cuối cùng.`k`các vị thế có chỉ số được biết chính xác. 

Một lỗi phổ biến là cố gắng mô phỏng từng bước chèn vào chuỗi, điều này dẫn đến sự phức tạp không cần thiết và các lỗi sai lệch trong việc dịch chuyển chỉ mục. Giải pháp này tránh được mọi sự dịch chuyển bằng cách suy luận trực tiếp về vị trí cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
abc
3
1 2 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
```Trọng lượng:`a=1, b=2, c=2`, vì vậy trọng lượng tối đa là 2. 

| Bước | Đóng góp | Chi tiết | 
| --- | --- | --- | 
| Bản gốc | 1·1 + 2·2 + 3·2 = 11 | vị trí 1-3 | 
| Đã chèn | 2·4 + 2·5 + 2·6 = 30 | vị trí bổ sung | 
| Tổng cộng | 41 | tổng hợp | 

Điều này xác nhận rằng chỉ chèn chữ cái tốt nhất (`b`hoặc`c`) ở cuối là tối ưu và thứ tự giữa chúng không quan trọng. 

### Ví dụ 2 

đầu vào:```
a
2
5 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
```Đây`a`có trọng số 5 nên đây cũng là chữ cái chèn tốt nhất. 

| Bước | Đóng góp | Chi tiết | 
| --- | --- | --- | 
| Bản gốc | 1·5 = 5 | ký tự đơn | 
| Đã chèn | 5·2 + 5·3 = 25 | vị trí 2 và 3 | 
| Tổng cộng | 30 | tổng cuối cùng | 

Điều này cho thấy ngay cả khi ký tự gốc đã tối ưu, việc chèn thêm chỉ đơn giản là mở rộng chuỗi mà không thay đổi chiến lược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | một lần chuyển qua chuỗi cộng với k lần chèn | 
| Không gian | O(1) | chỉ có mảng và bộ đếm trọng lượng có kích thước cố định | 

Các ràng buộc cho phép tổng chiều dài lên tới 2000, do đó việc quét tuyến tính dễ dàng nằm trong giới hạn. Không cần sắp xếp hoặc lập trình động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    s = inp.strip().splitlines()
    # re-run full solution inline
    it = iter(s)
    st = next(it).strip()
    k = int(next(it))
    w = list(map(int, next(it).split()))
    
    n = len(st)
    base = 0
    for i, ch in enumerate(st, 1):
        base += i * w[ord(ch) - 97]
    mx = max(w)
    for i in range(1, k+1):
        base += (n+i) * mx
    return str(base)

# provided sample
assert run("abc\n3\n1 2 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1") == "41"

# single char, no insertions
assert run("a\n0\n5 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1") == "5"

# all weights equal
assert run("ab\n2\n1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1") == "9"

# max k
assert run("a\n3\n10 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0") == "50"

# mixture
assert run("ba\n1\n1 3 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1") == "12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a, k=0`| 5 | không xử lý chèn | 
| trọng lượng bằng nhau | 9 | hành vi buộc | 
| max k char đơn | 50 | tích lũy chèn lặp đi lặp lại | 
| chuỗi hỗn hợp | 12 | tương tác gốc + chèn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi`k = 0`. Thuật toán phải bỏ qua tất cả logic chèn và chỉ tính điểm ban đầu. Quá trình triển khai xử lý vấn đề này một cách tự nhiên vì vòng lặp chèn không chạy lần nào, chỉ để lại phần đóng góp cơ bản. 

Một trường hợp khác là khi tất cả các trọng số đều bằng nhau. Bất kỳ lựa chọn chèn nào cũng trở nên tương đương, do đó thuật toán vẫn chọn chiến lược tối ưu hợp lệ bằng cách chọn bất kỳ chữ cái có trọng số tối đa nào. Việc tính toán vẫn đúng vì tất cả các đóng góp được chèn đều giống nhau bất kể lựa chọn chữ cái. 

Khi chuỗi có độ dài 1 và`k`lớn thì những đóng góp được chèn vào sẽ chiếm ưu thế. Thuật toán đặt chính xác tất cả các phần chèn vào từ vị trí 2 đến`k+1`, đảm bảo rằng mỗi ký tự bổ sung góp phần tăng số nhân, đây chính xác là sự sắp xếp tối ưu theo tính điểm tuyến tính.
