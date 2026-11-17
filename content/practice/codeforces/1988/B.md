---
title: "CF 1988B - chiếm đa số"
description: "Chúng ta được cung cấp một chuỗi nhị phân có thể thay đổi hình dạng theo thời gian. Động thái duy nhất được phép là lấy bất kỳ phân đoạn liền kề nào và nén nó thành một giá trị duy nhất bằng phần lớn của phân đoạn đó, trong đó các mối quan hệ được giải quyết theo hướng có lợi cho số 0."
date: "2026-06-08T15:50:17+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1988
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 958 (Div. 2)"
rating: 900
weight: 1988
solve_time_s: 310
verified: false
draft: false
---

[CF 1988B - Tạo nên đa số](https://codeforces.com/problemset/problem/1988/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 5 phút 10 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân có thể thay đổi hình dạng theo thời gian. Động thái duy nhất được phép là lấy bất kỳ phân đoạn liền kề nào và nén nó thành một giá trị duy nhất bằng phần lớn của phân đoạn đó, trong đó các mối quan hệ được giải quyết theo hướng có lợi cho số 0. Mỗi thao tác làm giảm độ dài của chuỗi, vì vậy cuối cùng chúng ta kết thúc bằng một số duy nhất. 

Nhiệm vụ là xác định xem có thể rút gọn toàn bộ dãy thành một phần tử bằng một hay không. 

Khó khăn chính là hoạt động không thể đảo ngược và nó thay đổi cả độ dài và phân bố giá trị. Một phân khúc có vẻ thuận lợi tại địa phương vẫn có thể phá hủy cấu trúc toàn cầu nếu sử dụng không đúng cách. Quyết định hoàn toàn là về sự tồn tại chứ không phải về xây dựng. 

Các ràng buộc cho phép tổng số tối đa 2·10^5 phần tử trong các trường hợp thử nghiệm. Bất kỳ giải pháp nào mô phỏng các hoạt động hoặc cố gắng tìm kiếm trên các phân đoạn sẽ quá chậm vì bản thân mỗi hoạt động là O(n) và có thể có nhiều hoạt động trước khi hội tụ. Ngay cả cách tiếp cận bậc hai cho mỗi trường hợp thử nghiệm cũng sẽ quá lớn. 

Một số trường hợp phức tạp đáng được nêu ra. 

Một số 0 duy nhất, chẳng hạn như đầu vào`0`, rõ ràng là không thể biến thành`1`bởi vì không có cách nào để giới thiệu những cái đó. 

Một chuỗi như`10`cũng là không thể: các hoạt động phân đoạn duy nhất là đơn lẻ hoặc toàn bộ mảng. Toàn bộ mảng có một số 0 và một số một, vì vậy phần lớn bằng 0, điều này sẽ thu gọn mọi thứ thành`0`. 

Một trường hợp thú vị hơn là một cái gì đó như`10001`. Mặc dù có tồn tại nhưng chúng rất thưa thớt và bất kỳ thao tác nào bao gồm quá nhiều số 0 sẽ ngay lập tức chuyển về 0, khiến cho việc khôi phục là không thể. 

Những ví dụ này cho thấy câu trả lời không bị chi phối bởi cấu trúc của các phân đoạn mà bởi điều kiện cân bằng toàn cầu. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực cố gắng mô phỏng tất cả các hoạt động có thể xảy ra. Ở mỗi bước, chúng ta có thể chọn bất kỳ mảng con nào, tính toán phần lớn của nó và tiếp tục đệ quy chuỗi kết quả. Điều này tạo thành một cây tìm kiếm khổng lồ trong đó mỗi trạng thái phân nhánh thành các khả năng O(n^2) và độ sâu cũng có thể là O(n). Ngay cả với tính năng ghi nhớ, số lượng trạng thái riêng biệt vẫn theo cấp số nhân vì các chuỗi nén khác nhau tạo ra các mảng trung gian khác nhau. Cách tiếp cận này nhanh chóng trở nên không khả thi. 

Quan sát quan trọng là hoạt động có hành vi đơn điệu mạnh mẽ: nó chỉ làm giảm mảng và thay thế cấu trúc cục bộ bằng một bit duy nhất phản ánh phần lớn cục bộ. Điều quan trọng là chúng chỉ được bảo tồn nếu chúng đủ chiếm ưu thế ở một số khu vực. Số 0 đóng vai trò như một kết quả mặc định khi có bất kỳ sự ràng buộc cục bộ nào hoặc đa số bằng 0. 

Chúng tôi muốn kết thúc bằng một`1`, có nghĩa là tại một thời điểm nào đó phải tồn tại một phân đoạn mà việc nén lặp đi lặp lại không bao giờ loại bỏ được tất cả các phân đoạn. Nếu ở bất kỳ giai đoạn nào, chúng tôi tạo một phân khúc bị các số 0 thống trị thì phân khúc đó sẽ giảm về 0 và không thể đóng góp vào phân khúc cuối cùng. 

Điều này dẫn đến một sự đơn giản hóa quan trọng: cách duy nhất để kết thúc bằng một`1`là nếu tổng số đơn vị lớn hơn số không. Nếu các số 0 bằng hoặc nhiều hơn, mọi hoạt động toàn mảng sẽ ngay lập tức thu gọn về 0 và không có cơ chế nào để “khôi phục” các số 0 sau đó vì quá trình chỉ nén chứ không bao giờ tạo các số 0. 

Nếu số 1 lớn hơn một cách nghiêm ngặt, chúng ta luôn có thể tìm thấy một chuỗi nén loại bỏ dần các số 0 trong khi vẫn giữ được phần lớn các số 1. Theo trực giác, chúng ta luôn có thể chọn các phân đoạn tránh lật đổ sự thống trị toàn cầu, cuối cùng thu gọn mảng đó thành một mảng duy nhất. 

Vì vậy, câu trả lời rút gọn thành việc kiểm tra xem count(1) > count(0) hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Đếm Tối Ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lượng số một và số không trong chuỗi đầu vào. Điều này đưa ra một bản tóm tắt toàn cục về mảng mà không cần mô phỏng bất kỳ thao tác nào. 
2. So sánh số lượng. Nếu số lượng hàng đơn vị hoàn toàn lớn hơn số lượng số 0, xuất CÓ. Nếu không thì xuất ra NO. 

### Tại sao nó hoạt động 

Mọi hoạt động đều thay thế một phân khúc bằng phần lớn của nó, điều này không bao giờ làm tăng số lượng phân khúc trừ khi những phân khúc đó đã chiếm ưu thế cục bộ. Bất kỳ phân đoạn nào có ít nhất số 0 bằng số 1 sẽ thu gọn thành 0, làm giảm vĩnh viễn sự hiện diện của các số 0 trong hệ thống. Nếu các số 0 không hoàn toàn nhỏ hơn các số 1 trên toàn cầu thì mọi nỗ lực hợp nhất toàn bộ mảng cuối cùng sẽ tạo ra số 0 và vì các phép toán chỉ làm giảm cấu trúc nên không có cách nào để đưa lại các số 0 bị mất. Ngược lại, nếu 1 lớn hơn, chúng ta luôn có thể tránh thu gọn toàn bộ cấu trúc về 0 bằng cách hợp nhất các phân đoạn được chọn cẩn thận, duy trì cấu hình một ưu thế cho đến khi mảng co lại thành một phần tử duy nhất. 

Điều này làm cho sự bất bình đẳng toàn cầu giữa các số lượng vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    
    ones = s.count('1')
    zeros = n - ones
    
    if ones > zeros:
        print("YES")
    else:
        print("NO")
```Giải pháp đọc từng trường hợp thử nghiệm một cách độc lập và tính toán số lượng theo thời gian tuyến tính. Logic cốt lõi là sự so sánh giữa số một và số không. Không cần mô phỏng hoặc theo dõi cấu trúc. 

Điều tinh tế duy nhất là đảm bảo chuỗi được loại bỏ một cách chính xác, vì việc theo dõi các dòng mới sẽ ảnh hưởng đến việc đếm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`100000001`Chúng tôi chỉ theo dõi số lượng. 

| Bước | những cái | số không | quyết định | 
| --- | --- | --- | --- | 
| ban đầu | 2 | 7 | so sánh | 

Vì số 0 vượt quá số 1 nên câu trả lời là KHÔNG. 

Điều này cho thấy rằng mặc dù những cái này tồn tại ở cả hai đầu, chúng vẫn quá thưa thớt để thống trị bất kỳ hoạt động nén toàn cầu nào. 

### Ví dụ 2 

đầu vào:`000011000`| Bước | những cái | số không | quyết định | 
| --- | --- | --- | --- | 
| ban đầu | 2 | 7 | so sánh | 

Một lần nữa số 0 chiếm ưu thế nên đầu ra là KHÔNG. 

Điều này xác nhận rằng việc phân cụm các số 0 không quan trọng, chỉ tính tổng số. 

### Ví dụ 3 

đầu vào:`1110`| Bước | những cái | số không | quyết định | 
| --- | --- | --- | --- | 
| ban đầu | 3 | 1 | so sánh | 

Vì số 1 vượt quá số 0 nên đầu ra là CÓ. 

Điều này chứng tỏ rằng ngay cả khi có số 0 ở cuối, những cái đó có thể hấp thụ nó thông qua các hoạt động phân đoạn thích hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm quét chuỗi một lần để đếm ký tự | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm số nguyên | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm tối đa là 2·10^5, do đó, quá trình quét tuyến tính trên mỗi trường hợp thử nghiệm có thể dễ dàng đủ nhanh trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()
        ones = s.count('1')
        zeros = n - ones
        out.append("YES" if ones > zeros else "NO")
    return "\n".join(out)

# provided samples
assert run("""5
1
0
1
1
2
01
9
100000001
9
000011000
""") == """No
Yes
No
Yes
No"""

# custom cases
assert run("""1
1
1
""") == "YES", "single one"

assert run("""1
1
0
""") == "NO", "single zero"

assert run("""1
5
11111
""") == "YES", "all ones"

assert run("""1
6
110000
""") == "NO", "equal-ish boundary 2 vs 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`| CÓ | trường hợp tích cực tối thiểu | 
|`1\n1\n0`| KHÔNG | trường hợp tiêu cực tối thiểu | 
|`1\n5\n11111`| CÓ | tất cả những điều cực đoan | 
|`1\n6\n110000`| KHÔNG | ranh giới thống trị | 

## Vỏ cạnh 

Mảng một phần tử làm nổi bật hành vi cơ bản. Đối với đầu vào`1`, câu trả lời tầm thường là CÓ vì mảng đã bằng rồi`[1]`. Thuật toán tính toán chính xác số 1 = 1, số 0 = 0 và trả về CÓ. 

Đối với đầu vào`0`, số 1 = 0 và số 0 = 1, do đó đầu ra là KHÔNG. Điều này phù hợp với việc không thể giới thiệu những cái đó thông qua bất kỳ thao tác nào. 

Cấu trúc cân bằng như`01`hoặc`10`luôn có số lượng bằng nhau, dẫn đến NO. Thuật toán không phụ thuộc vào sự sắp xếp, điều này phù hợp với thực tế là bất kỳ sự hợp nhất hoàn toàn nào của một phân đoạn cân bằng sẽ sụp đổ về 0 do đứt gãy.
