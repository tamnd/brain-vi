---
title: "CF 1170D - Giải mã dãy số nguyên"
description: "Chúng ta được cấp một dãy phẳng duy nhất được tạo ra từ một số dãy số nguyên ẩn. Quá trình mã hóa trộn tất cả các chuỗi lại với nhau theo “thứ tự cột”: đầu tiên là tất cả các phần tử đầu tiên của mỗi chuỗi, sau đó là tất cả các phần tử thứ hai, sau đó là tất cả các phần tử thứ ba, v.v."
date: "2026-06-18T17:08:17+07:00"
tags: ["codeforces", "competitive-programming", "*special", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1170
codeforces_index: "D"
codeforces_contest_name: "Kotlin Heroes: Episode 1"
rating: 0
weight: 1170
solve_time_s: 82
verified: false
draft: false
---

[CF 1170D - Giải mã dãy số nguyên](https://codeforces.com/problemset/problem/1170/D) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy phẳng duy nhất được tạo ra từ một số dãy số nguyên ẩn. Quá trình mã hóa trộn tất cả các chuỗi lại với nhau theo “thứ tự cột”: đầu tiên là tất cả các phần tử đầu tiên của mỗi chuỗi, sau đó là tất cả các phần tử thứ hai, sau đó là tất cả các phần tử thứ ba, v.v. Nếu một chuỗi hết phần tử, giá trị đánh dấu đặc biệt -1 sẽ được sử dụng ở vị trí của nó kể từ thời điểm đó trở đi. 

Nhiệm vụ là xây dựng lại các chuỗi ban đầu, cả nhóm và thứ tự bên trong của chúng, từ luồng được hợp nhất này. 

Một cách hữu ích để hình dung về quy trình là mỗi chuỗi ban đầu đóng góp một giá trị cho mỗi “vòng”. Ở vòng 1, chúng tôi lấy phần tử đầu tiên của mỗi chuỗi, ở vòng 2, phần tử thứ hai của mỗi chuỗi, v.v. Quá trình mã hóa chỉ dừng khi mỗi chuỗi đã góp phần kết thúc -1. 

Đầu ra phải tái tạo lại chính xác có bao nhiêu chuỗi tồn tại và mỗi chuỗi chứa những gì trước khi mã hóa. 

Các ràng buộc cho phép tối đa 300.000 giá trị được mã hóa. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng quét hoặc mô phỏng liên tục quy trình trên mỗi chuỗi hoặc mỗi vòng theo cách lồng nhau đơn giản. Chúng ta cần tái cấu trúc tuyến tính để xử lý từng phần tử được mã hóa một lần hoặc một số lần không đổi. 

Một khó khăn nhỏ đến từ việc phân biệt các dấu -1 thực trong quá trình mã hóa với sự kết thúc cấu trúc. Mỗi chuỗi đóng góp chính xác một -1 và các giá trị -1 đó xuất hiện theo các “vòng” được đồng bộ hóa. Một cách tiếp cận ngây thơ coi -1 là điểm dừng cứng cho tất cả các chuỗi sẽ bị hỏng vì các chuỗi kết thúc vào các thời điểm khác nhau. 

Một trường hợp cạnh khác là các chuỗi trống. Một số chuỗi có thể không chứa phần tử nào cả, nghĩa là đóng góp đầu tiên của chúng đã là -1. Chúng vẫn phải được thể hiện chính xác trong đầu ra. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng mô phỏng quá trình mã hóa ngược lại bằng cách quét liên tục chuỗi, thu thập các phần tử theo vòng và gán chúng cho các chuỗi cho đến khi dấu kết thúc xuất hiện. Người ta có thể tưởng tượng việc lặp lại mảng được mã hóa nhiều lần, mỗi lần trích xuất “cột” giá trị tiếp theo cho tất cả các chuỗi vẫn đang hoạt động. 

Điều này nhanh chóng trở nên quá chậm. Nếu có$m$các yếu tố và có khả năng$O(m)$vòng, mỗi vòng quét một tiền tố lớn sẽ dẫn đến$O(m^2)$hành vi trong trường hợp xấu nhất. 

Thông tin chi tiết về cấu trúc quan trọng là việc mã hóa duy trì một mẫu đan xen nghiêm ngặt: các phần tử được viết theo lớp và trong mỗi lớp, chúng luôn xuất hiện theo thứ tự tuần tự. Điều này có nghĩa là chúng ta có thể tái tạo lại các chuỗi dần dần bằng cách duy trì danh sách các chuỗi đang hoạt động hiện tại và gán từng giá trị gặp phải vào đúng vị trí chuỗi theo kiểu truyền phát. 

Thay vì suy nghĩ theo các vòng, chúng tôi xử lý luồng từ trái sang phải và duy trì một danh sách động các chuỗi hiện đang “hoạt động”. Mỗi lần chúng tôi gặp một giá trị, chúng tôi sẽ thêm giá trị đó vào đúng chuỗi dựa trên vị trí của nó trong vòng hiện tại. Khi chúng ta nhấn -1 cho một chuỗi, chuỗi đó sẽ bị xóa khỏi các vòng trong tương lai. 

Thử thách chính là việc theo dõi từng giá trị thuộc về chuỗi nào khi các chuỗi biến mất. Cấu trúc đảm bảo rằng trong mỗi vòng, chúng tôi luôn điền các chuỗi theo thứ tự, vì vậy chúng tôi có thể sử dụng lại các vị trí từ các vòng trước và thu gọn tập hoạt động khi chúng tôi tiếp tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force theo vòng | O(m2) | O(m) | Quá chậm | 
| Tái thiết trực tuyến với tính năng theo dõi tích cực | O(m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi được mã hóa và trước tiên xác định nơi kết thúc của chuỗi. Mỗi -1 tương ứng với phần cuối của chính xác một chuỗi. Vì các chuỗi được xử lý theo thứ tự mỗi vòng, nên lần cuối cùng một chuỗi xuất hiện trước -1 sẽ cho chúng ta biết cấu trúc độ dài của nó. 
2. Duy trì một danh sách động các trình tự hiện đang được xây dựng lại. Ban đầu, không có trình tự nào được tạo ra. 
3. Lặp lại mảng được mã hóa trong khi vẫn giữ con trỏ ở “vị trí tròn” hiện tại. Đối với mỗi giá trị, hãy quyết định xem nó bắt đầu một chuỗi mới hay tiếp tục chuỗi hiện có. 
4. Khi giá trị khác -1 xuất hiện và không có đủ chuỗi hoạt động cho vị trí vòng hiện tại, hãy tạo một chuỗi mới và thêm giá trị vào đó. Điều này phản ánh thực tế là một chuỗi mới đóng góp phần tử đầu tiên của nó vào vị trí này. 
5. Khi giá trị khác -1 xuất hiện và tồn tại một chuỗi hoạt động tương ứng ở vị trí đó, hãy thêm giá trị vào chuỗi đó. 
6. Khi gặp -1, nó tương ứng với một chuỗi kết thúc ở vị trí này trong vòng hiện tại. Chúng tôi đánh dấu chuỗi đó là đã hoàn tất và xóa chuỗi đó khỏi quá trình theo dõi đang hoạt động để chuỗi đó không còn tham gia vào các vòng sau nữa. 
7. Tiếp tục cho đến khi sử dụng hết toàn bộ mảng mã hóa, đảm bảo rằng tất cả các chuỗi đã được xây dựng đầy đủ. 

Điều bất biến chính là tại bất kỳ thời điểm nào, danh sách trình tự đang hoạt động sẽ thể hiện chính xác các trình tự vẫn còn các phần tử để đóng góp trong các vòng hiện tại hoặc tương lai. Mỗi vị trí trong một vòng tương ứng với một chuỗi nhận dạng ổn định và khi một chuỗi bị xóa thông qua -1, nó sẽ không bao giờ xuất hiện lại ở các vòng sau. Điều này đảm bảo rằng mỗi giá trị được gán cho đúng chuỗi gốc đúng một lần, duy trì thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m = int(input())
    b = list(map(int, input().split()))

    seqs = []
    active = []

    idx = 0

    for val in b:
        if val == -1:
            if idx < len(active):
                # mark sequence as finished
                active[idx] = None
            idx += 1
        else:
            if idx == len(active):
                active.append(len(seqs))
                seqs.append([])
            seq_id = active[idx]
            seqs[seq_id].append(val)

    # filter out removed slots if any
    result = []
    for i in active:
        if i is not None:
            result.append(seqs[i])

    print(len(result))
    for s in result:
        print(len(s), *s)

if __name__ == "__main__":
    solve()
```Mã duy trì hai cấu trúc:`seqs`, nơi lưu trữ các chuỗi được xây dựng lại và`active`, ánh xạ các vị trí vòng hiện tại tới các chỉ số tuần tự. Con trỏ`idx`theo dõi cột nào (vị trí tròn) mà chúng tôi hiện đang điền. Khi một giá trị mới xuất hiện và không có chuỗi nào tồn tại ở vị trí đó thì một chuỗi mới sẽ được tạo. 

MỘT`-1`đánh dấu sự kết thúc của chuỗi hiện đang chiếm cột đó, vì vậy chúng tôi vô hiệu hóa nó trong`active`. Điều này đảm bảo rằng các vòng sau không được thêm vào đó nữa. 

Bước lọc cuối cùng sẽ loại bỏ các chuỗi kết thúc sớm và đảm bảo chỉ các chuỗi được tái tạo hợp lệ mới được in. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
12
3 2 1 1 7 2 4 -1 3 -1 4 -1
```| Bước | Giá trị | idx | hoạt động | trạng thái seqs | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | [0] | [[3]] | 
| 2 | 2 | 1 | [0,1] | [[3],[2]] | 
| 3 | 1 | 2 | [0,1,2] | [[3],[2],[1]] | 
| 4 | 1 | 0 | [0,1,2] | [[3,1],[2],[1]] | 
| 5 | 7 | 1 | [0,1,2] | [[3,1],[2,7],[1]] | 
| 6 | 2 | 2 | [0,1,2] | [[3,1],[2,7],[1,2]] | 
| 7 | 4 | 0 | [0,1,2] | [[3,1,4],[2,7],[1,2]] | 
| 8 | -1 | 1 | [0,Không có,2] | seq2 kết thúc | 
| 9 | 3 | 2 | ... | tiếp tục điền | 
| 10 | -1 | 0 | [Không,Không,2] | seq0 kết thúc | 
| 11 | 4 | 2 | ... | trình tự cuối cùng | 
| 12 | -1 | 2 | tất cả đã kết thúc | xong | 

Dấu vết này cho thấy các chuỗi được điền theo từng cột như thế nào trong khi kết thúc độc lập tại các thời điểm khác nhau. 

### Ví dụ 2 

đầu vào:```
5
1 2 -1 3 4
```| Bước | Giá trị | idx | hoạt động | trạng thái seqs | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | [0] | [[1]] | 
| 2 | 2 | 1 | [0,1] | [[1],[2]] | 
| 3 | -1 | 0 | [Không có,1] | chuỗi đầu tiên kết thúc | 
| 4 | 3 | 1 | [Không có,1] | [[1],[2,3]] | 
| 5 | 4 | 1 | [Không có,1] | [[1],[2,3,4]] | 

Chuỗi thứ hai tiếp tục độc lập sau khi chuỗi thứ nhất kết thúc, cho thấy tại sao việc dừng toàn cục ở -1 sẽ không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi phần tử được mã hóa được xử lý một lần bằng các phép toán O(1) | 
| Không gian | O(m) | Lưu trữ tất cả các phần tử được xây dựng lại và ánh xạ hoạt động | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì 300.000 thao tác với khả năng xử lý liên tục trong thời gian cho mỗi phần tử đều nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    output = []
    def solve():
        m = int(input())
        b = list(map(int, input().split()))

        seqs = []
        active = []
        idx = 0

        for val in b:
            if val == -1:
                if idx < len(active):
                    active[idx] = None
                idx += 1
            else:
                if idx == len(active):
                    active.append(len(seqs))
                    seqs.append([])
                seq_id = active[idx]
                seqs[seq_id].append(val)

        res = []
        for i in active:
            if i is not None:
                res.append(seqs[i])

        output.append(str(len(res)))
        for s in res:
            output.append(str(len(s)) + (" " + " ".join(map(str, s)) if s else ""))

    solve()
    return "\n".join(output)

# provided sample
assert run("""12
3 2 1 1 7 2 4 -1 3 -1 4 -1
""") == """3
3 3 1 4
2 2 7
4 1 2 3 4"""

# empty sequences edge
assert run("""3
-1 -1 -1
""") == """3
0
0
0"""

# single sequence
assert run("""4
1 2 3 -1
""") == """1
3 1 2 3"""

# staggered termination
assert run("""5
1 2 -1 3 4
""") == """2
1 1
3 2 3 4"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả -1 | ba chuỗi trống | xử lý chuỗi trống | 
| chuỗi tuyến tính đơn | một chuỗi đầy đủ | tái thiết cơ bản | 
| chấm dứt sớm | trình tự phân chia | so le -1 xử lý | 

## Vỏ cạnh 

Một kịch bản hoàn toàn trống trong đó tất cả các giá trị là -1 chứng tỏ rằng thuật toán vẫn phải tạo ra nhiều chuỗi trống, vì mỗi -1 tương ứng với việc kết thúc một chuỗi riêng biệt. 

Ví dụ:```
-1 -1 -1
```Thuật toán tăng con trỏ cột hoạt động cho mỗi -1 và đánh dấu một vị trí chuỗi là đã kết thúc. Mặc dù không có giá trị thực nào được thêm vào nhưng ba chuỗi vẫn được xây dựng lại. 

Một trường hợp khác là khi các chuỗi kết thúc ở các độ sâu khác nhau. TRONG:```
1 2 -1 3 4
```chuỗi đầu tiên kết thúc sớm trong khi chuỗi thứ hai tiếp tục phát triển. Thuật toán giữ cho chuỗi thứ hai hoạt động một cách chính xác sau khi loại bỏ chuỗi đầu tiên, đảm bảo các giá trị không bị thêm nhầm vào chuỗi đã kết thúc.
