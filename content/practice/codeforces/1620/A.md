---
title: "CF 1620A – Bằng hoặc Không Bằng"
description: "Chúng ta được cho một mảng hình tròn gồm các số nguyên dương và với mỗi cặp số liền kề trong hình tròn, chúng ta biết các số đó có bằng nhau hay không."
date: "2026-06-10T06:02:58+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dsu", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1620
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 119 (Rated for Div. 2)"
rating: 800
weight: 1620
solve_time_s: 221
verified: false
draft: false
---

[CF 1620A - Bằng hoặc Không bằng](https://codeforces.com/problemset/problem/1620/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, dsu, triển khai 
**Thời gian giải:** 3 phút 41s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng hình tròn gồm các số nguyên dương và với mỗi cặp số liền kề trong hình tròn, chúng ta biết các số đó có bằng nhau hay không. Đầu vào cung cấp thông tin này dưới dạng một chuỗi`s`của các nhân vật`E`Và`N`, Ở đâu`E`chỉ ra hai hàng xóm phải bằng nhau và`N`cho biết chúng phải khác nhau. Thách thức ở đây là quyết định xem có thể gán các số thực tế cho mảng thỏa mãn tất cả các đẳng thức và bất đẳng thức này hay không. Chúng ta không cần xây dựng mảng, chỉ cần trả lời CÓ hoặc KHÔNG cho từng trường hợp thử nghiệm. 

Các ràng buộc nhỏ: độ dài mảng`n`dao động từ 2 đến 50 và có thể có tới 1000 trường hợp thử nghiệm. Từ`n`nhỏ, nên có thể chấp nhận một thuật toán kiểm tra tất cả các cặp hoặc thực hiện nhiều lần tuyến tính cho mỗi trường hợp kiểm thử. Khó khăn cốt lõi là lý luận về chuỗi bình đẳng trong một cấu trúc vòng tròn, trong đó`E`các ràng buộc lan truyền trên nhiều phần tử và`N`các ràng buộc gây ra xung đột nếu chúng mâu thuẫn với các chuỗi này. 

Một trường hợp quan trọng phát sinh khi tất cả các mối quan hệ`N`. Ví dụ, đối với`n = 3`Và`s = NNN`, mỗi phần tử phải khác với các phần tử lân cận của nó. Điều này là không thể vì vòng tròn khép lại và một phần tử sẽ phải bằng phần tử khác để thỏa mãn phần tử cuối cùng.`N`. Việc triển khai bất cẩn có thể bỏ qua việc đóng vòng tròn và xuất ra sai CÓ. Một trường hợp tế nhị khác xảy ra khi chỉ có một`N`được bao quanh bởi`E`s: cái`E`chuỗi có thể hấp thụ các yếu tố, nhưng một chuỗi duy nhất`N`ở ranh giới có thể tạo ra một xung đột không thể tránh khỏi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử tất cả các phép gán số có thể có cho đến giới hạn lớn nào đó (chẳng hạn như 10^9). Đối với mỗi nhiệm vụ, chúng tôi kiểm tra xem mọi`E`trận đấu và mọi`N`khác nhau. Điều này đúng nhưng không thực tế: vì`n = 50`, việc thử thậm chí 2 giá trị cho mỗi phần tử yêu cầu 2^50 phép gán, vượt xa tính toán khả thi. 

Cách tiếp cận tối ưu xử lý vấn đề như một phép gán mang tính xây dựng của hai giá trị có thể. Chúng ta có thể gán nhãn cho từng phần tử, truyền bá các ràng buộc đẳng thức dọc theo vòng tròn. Nếu hai phần tử được kết nối bởi`E`, chúng phải có chung nhãn. Nếu kết nối bằng`N`, chúng phải khác nhau. Thông tin chi tiết quan trọng là chúng ta chỉ cần hai nhãn khác nhau, bởi vì mỗi`N`ràng buộc chỉ yêu cầu hai điểm cuối khác nhau. Nếu chuỗi`s`bao gồm hoàn toàn`E`s, chúng ta có thể gán tất cả các phần tử cùng một giá trị. Nếu không, bất kỳ sự sắp xếp nào của`N`s có thể được giải quyết miễn là có ít nhất một`N`và chuỗi có thể bắt đầu từ thời điểm đó. Trường hợp duy nhất không thể xảy ra là khi`n = 3`và cả ba ràng buộc đều là`N`hoặc các mẫu khác trong đó mọi phần tử phải khác với mọi phần tử lân cận trong vòng tròn có kích thước 3. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Hai nhãn mang tính xây dựng | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`và lặp lại từng trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp một chuỗi`s`chiều dài`n`. 
2. Nếu`s`chỉ chứa`E`s, in ngay CÓ. Điều này hợp lệ vì chúng ta có thể gán cùng một số cho tất cả các phần tử trong vòng tròn. 
3. Đối với các chuỗi khác, chúng ta kiểm tra trường hợp đặc biệt không thể xảy ra khi`n = 3`và tất cả các nhân vật đều`N`. Trong tình huống này, mỗi phần tử phải khác với các phần tử lân cận của nó, điều này là không thể trong một vòng tròn có độ dài 3. 
4. Trong tất cả các trường hợp khác, chúng ta có thể xây dựng một phép gán hợp lệ bằng cách gán hai số khác nhau luân phiên bắt đầu từ số đầu tiên.`N`hạn chế. Nếu hàng xóm phải bằng nhau, hãy truyền bá nhãn giống nhau; nếu hàng xóm phải khác, hãy chuyển nhãn. 
5. Vì chúng ta chỉ cần xác định tính khả thi và không xây dựng mảng một cách rõ ràng, nên chúng ta có thể kết luận CÓ nếu tránh được trường hợp 3 phần tử toàn N không thể thực hiện được. 
6. In KHÔNG cho trường hợp không thể và CÓ nếu ngược lại. 

Tại sao nó hoạt động: Bằng cách coi các đẳng thức là các thành phần được kết nối và các hiệu là các ràng buộc giữa các thành phần, chúng ta có thể thấy rằng bất kỳ chuỗi nào có độ dài lớn hơn 3 với ít nhất một đẳng thức hoặc hiệu đều có thể được thỏa mãn bằng hai số phân biệt. Việc truyền bá sự bình đẳng đảm bảo rằng các chuỗi`E`s là nhất quán và có thể thỏa mãn sự khác biệt bằng cách chuyển đổi giữa hai nhãn. Thuật toán không bao giờ tạo ra CÓ sai vì nó xử lý rõ ràng xung đột toàn N vòng tròn 3 phần tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        n = len(s)
        if s.count('E') == n:
            print("YES")
        elif n == 3 and s == "NNN":
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc đầu vào và lặp lại các trường hợp thử nghiệm. Kiểm tra`s.count('E') == n`xử lý trường hợp hoàn toàn bằng nhau một cách hiệu quả. Kiểm tra đặc biệt`n == 3 and s == "NNN"`nắm bắt được sự sắp xếp bất khả thi duy nhất cho một vòng tròn tối thiểu trong đó mọi hàng xóm đều phải khác nhau. Tất cả các sắp xếp khác đều tự động khả thi, vì vậy chúng tôi in CÓ. Việc xử lý ranh giới là tự động vì độ dài chuỗi được sử dụng trực tiếp từ đầu vào và việc đóng vòng tròn chỉ liên quan đến mẫu không thể thực hiện được mà chúng tôi kiểm tra rõ ràng. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
EEE
```| Bước | s | n | Hành động | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | EE | 3 | tất cả E | CÓ | 

Tất cả các phần tử có thể bằng nhau, vì vậy kết quả đầu ra là CÓ. 

Đối với đầu vào:```
EN
```| Bước | s | n | Hành động | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | VN | 2 | n != 3, không phải tất cả E | CÓ | 

Mặc dù một phần tử khác với phần tử kia, nhưng đường tròn 2 phần tử có thể thỏa mãn N, do đó đầu ra là CÓ. 

Đối với đầu vào:```
NNN
```| Bước | s | n | Hành động | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | NNN | 3 | n == 3 và tất cả N | KHÔNG | 

Không thể gán các số riêng biệt để thỏa mãn tất cả N nên đầu ra là NO. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi trường hợp kiểm thử yêu cầu quét chuỗi một lần, trường hợp xấu nhất là 50 cho mỗi trường hợp kiểm thử, tối đa 1000 trường hợp kiểm thử. | 
| Không gian | O(1) | Không cần cấu trúc bổ sung; chỉ các biến tạm thời cho mỗi trường hợp thử nghiệm. | 

Thuật toán thoải mái trong giới hạn thời gian và bộ nhớ đối với các ràng buộc nhất định. Hoạt động tối đa là 50.000, đó là chuyện nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("4\nEEE\nEN\nENNEENE\nNENN\n") == "YES\nYES\nYES\nYES", "sample 1"

# custom cases
assert run("1\nNNN\n") == "NO", "3-element all N"
assert run("1\nEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEE\n") == "YES", "50-element all E"
assert run("1\nNENENENENENENENENENENENENENENENENENENENENENENENEN\n") == "YES", "alternating N and E"
assert run("1\nEN\n") == "YES", "2-element one N one E"
assert run("1\nE\n") == "YES", "2-element all E implicit closure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| NNN | KHÔNG | vòng tròn không thể tối thiểu | 
| 50 giây | CÓ | kích thước tối đa hoàn toàn bằng nhau | 
| NENEN... | CÓ | ràng buộc xen kẽ | 
| VN | CÓ | vòng tròn nhỏ, ràng buộc hỗn hợp | 
| E | CÓ | vòng tròn tối thiểu đều bằng nhau | 

## Vỏ cạnh 

Đối với đầu vào`NNN`, thuật toán kiểm tra`n == 3 and s == "NNN"`và in KHÔNG, xử lý chính xác trường hợp không thể. Đối với một chuỗi có độ dài 50 với tất cả`E`là, việc kiểm tra số lượng sẽ kích hoạt và in ra CÓ. Để luân phiên`N`Và`E`, thuật toán in ra CÓ vì một vòng tròn 50 phần tử có thể được thỏa mãn với hai số được gán xen kẽ nhau, xác nhận tính bất biến lan truyền. Tất cả các trường hợp biên với`n = 2`được xử lý một cách tầm thường, như bất kỳ cấu hình nào của`E`hoặc`N`là khả thi.
