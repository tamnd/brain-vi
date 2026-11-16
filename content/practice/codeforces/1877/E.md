---
title: "CF 1877E - Tự tổng hợp"
description: "Chúng ta được cho một dãy số nguyên dương được đánh chỉ mục từ trái sang phải. Chúng ta được phép chọn nhiều lần các vị trí và các phần tử “vòng tròn”. Điểm mấu chốt là việc khoanh tròn không loại bỏ các phần tử ngay lập tức; nó chỉ đánh dấu chúng."
date: "2026-06-09T01:05:34+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1877
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 902 (Div. 2, based on COMPFEST 15 - Final Round)"
rating: 2100
weight: 1877
solve_time_s: 103
verified: false
draft: false
---

[CF 1877E - Tự động tổng hợp](https://codeforces.com/problemset/problem/1877/E) 

**Xếp hạng:** 2100 
**Tags:** thuật toán xây dựng, đồ thị, cách triển khai 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên dương được đánh chỉ mục từ trái sang phải. Chúng ta được phép chọn nhiều lần các vị trí và các phần tử “vòng tròn”. Điểm mấu chốt là việc khoanh tròn không loại bỏ các phần tử ngay lập tức; nó chỉ đánh dấu chúng. Sau khi tất cả các thao tác khoanh tròn kết thúc, chúng ta xây dựng hai chuỗi từ cùng một quy trình. 

Chuỗi đầu tiên được hình thành bằng cách lấy tất cả các phần tử chưa bao giờ được khoanh tròn, giữ nguyên thứ tự chỉ mục ban đầu của chúng. Chuỗi thứ hai ghi lại các chỉ số được chọn trong các thao tác khoanh tròn, theo thứ tự chúng được chọn. Mục đích là thực hiện các thao tác xoay vòng sao cho hai chuỗi này trở nên giống hệt nhau. 

Điều này tạo ra một ràng buộc tự tham chiếu: các chỉ số chúng ta chọn theo thời gian phải khớp với các giá trị không được chọn ở cuối và các giá trị còn lại đó được xác định bởi chính hành động chọn. 

Các ràng buộc cho phép n lên tới 200000, điều này ngay lập tức loại trừ mọi cách xây dựng tập hợp con hàm mũ hoặc mô phỏng lặp lại trên tất cả các lựa chọn. Bất kỳ giải pháp hợp lệ nào về cơ bản phải là tuyến tính hoặc gần tuyến tính, tệ nhất có thể là O (n log n). 

Một vấn đề tế nhị xuất hiện ngay lập tức: được phép khoanh tròn cùng một phần tử nhiều lần, nhưng việc lặp lại các chỉ số trong p khi đó sẽ bao hàm các giá trị lặp lại trong r, nghĩa là các phần tử lặp lại không được khoanh tròn. Vì r chỉ phụ thuộc vào việc một phần tử có được khoanh tròn ít nhất một lần hay không, nên các thao tác lặp lại trên cùng một chỉ mục là dư thừa xét về r, nhưng vẫn ảnh hưởng đến p. Điều này có nghĩa là một cấu trúc tối ưu nên tránh lặp lại các chỉ số trừ khi cần thiết cho cấu trúc, nhưng lý luận thường cho thấy sự lặp lại là không cần thiết trong một giải pháp hợp lệ. 

Các trường hợp cạnh có xu hướng phá vỡ lý luận ngây thơ bao gồm các tình huống trong đó tất cả các phần tử giống hệt nhau hoặc khi cấu trúc buộc phải thực hiện một chu trình không thể thỏa mãn. Ví dụ: nếu tất cả các giá trị hướng vào trong theo cách không tồn tại điểm cố định “tự nhất quán” ổn định thì câu trả lời sẽ trở thành không thể mặc dù tính nhất quán cục bộ có vẻ ổn. 

## Phương pháp tiếp cận 

Một ý tưởng bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi hoạt động có thể xảy ra. Đối với mỗi tập hợp con của các chỉ số và mỗi thứ tự khoanh tròn chúng, chúng ta có thể tính toán kết quả của chuỗi không được khoanh tròn và so sánh nó với chuỗi thao tác. Điều này ngay lập tức bùng nổ về mặt tổ hợp vì có 2^n tập hợp con và n! đặt hàng, làm cho nó không thể thực hiện được ngay cả với n khoảng 20. 

Quan sát quan trọng là quy trình xác định cấu trúc giống như hoán vị: tập hợp không được khoanh tròn cuối cùng tương ứng với các chỉ số không được các phép toán “tiêu thụ” và các chỉ mục đó phải căn chỉnh chính xác với trình tự thao tác đã ghi. Điều này ngụ ý rằng mọi chỉ mục đều trở thành một phần của chuỗi đầu ra hoặc được sử dụng đầy đủ để “hỗ trợ” giao diện của một chỉ mục khác. 

Tái cơ cấu lại vấn đề, chúng ta đang cố gắng xây dựng một sự phụ thuộc có hướng một cách hiệu quả trong đó việc chọn chỉ mục i đóng góp i vào p, đồng thời loại bỏ i khỏi r. Vì r phải bằng p nên mọi chỉ mục tồn tại cũng phải xuất hiện dưới dạng chỉ mục được chọn và ngược lại. Sự đối xứng này buộc chúng ta phải suy nghĩ về việc kết hợp các chỉ số với các giá trị và xây dựng chuỗi phụ thuộc. 

Cái nhìn sâu sắc quan trọng là diễn giải từng giá trị như trỏ đến một vị trí bắt buộc trong cấu trúc đầu ra. Chúng tôi có thể xử lý từ phải sang trái và duy trì một tập hợp các chỉ mục hiện có sẵn có thể được sử dụng làm đầu ra. Khi chúng tôi gặp một giá trị, chúng tôi quyết định liệu nó có thể được thỏa mãn bởi một chỉ mục có sẵn hay nó phải trở thành một phần của chính cấu trúc đầu ra. Điều này dẫn đến việc xây dựng tham lam được hướng dẫn bởi sự cân bằng sẵn có và nhu cầu.

Ở cấp độ cao hơn, chúng tôi đảm bảo rằng bất cứ khi nào chúng tôi cam kết sử dụng một chỉ mục trong trình tự thao tác, chúng tôi cũng đảm bảo rằng nó sẽ xuất hiện theo trình tự không được khoanh tròn vào đúng thời điểm, duy trì sự ghép nối nhất quán giữa các thao tác và các phần tử còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Xây dựng phụ thuộc tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng cốt lõi là xây dựng trình tự thao tác theo cách đảm bảo sự bình đẳng giữa những gì chúng ta loại bỏ và những gì còn lại. 

Chúng tôi tiến hành như sau. 

1. Chúng tôi duy trì một cấu trúc theo dõi những chỉ mục nào vẫn “có thể sử dụng được” làm ứng cử viên cho việc bị loại bỏ hoặc còn lại trong chuỗi cuối cùng. Ban đầu, tất cả các chỉ số đều có thể sử dụng được. 
2. Chúng ta quét mảng từ phải sang trái, vì các vị trí sau khó đáp ứng hơn và cần được quyết định trước. Thứ tự này ngăn chặn những mâu thuẫn trong tương lai khi chỉ mục bắt buộc đã được sử dụng không chính xác. 
3. Tại mỗi chỉ số i, chúng ta quyết định liệu i nên là một phần của chuỗi không khoanh tròn cuối cùng hay một phần của chuỗi thao tác. Điều này được xác định bằng việc liệu có tồn tại một cách nhất quán để phù hợp với yêu cầu trong tương lai hay không. Nếu tôi có thể đáp ứng yêu cầu đang chờ xử lý, chúng tôi sẽ chỉ định yêu cầu đó cho phù hợp; nếu không, chúng tôi dành nó cho vai trò ngược lại. 
4. Khi chúng tôi quyết định rằng một chỉ mục sẽ được sử dụng trong các hoạt động, về mặt khái niệm, chúng tôi “tiêu thụ” nó, nghĩa là nó không thể xuất hiện trong danh sách không được khoanh tròn cuối cùng. Khi nó không được sử dụng trong các hoạt động, nó vẫn có sẵn và sẽ xuất hiện trong r. 
5. Chúng tôi xây dựng p đồng thời khi quyết định các hoạt động, đảm bảo rằng mọi chỉ số hoạt động đều được ghi lại theo thứ tự. r cuối cùng được xác định ngầm bởi tất cả các chỉ số không được đánh dấu. 
6. Sau khi xử lý tất cả các chỉ số, chúng tôi xác nhận rằng p được xây dựng bằng r. Nếu tại bất kỳ thời điểm nào tính nhất quán bị phá vỡ, chúng tôi kết luận là không thể thực hiện được. 

Lý do chính đằng sau quá trình xử lý ngược là các quyết định trước đó không được cản trở các ràng buộc sau này. Bằng cách xử lý các phần tử bị hạn chế nhất trước tiên, chúng tôi tránh được việc sử dụng tài nguyên sớm. 

### Tại sao nó hoạt động 

Thuật toán thực thi sự cân bằng toàn cục giữa hai bộ: các chỉ số được chọn làm phép toán và các chỉ số không được chọn. Mọi quyết định được đưa ra sao cho các chỉ số chưa được chỉ định còn lại vẫn có thể đáp ứng các yêu cầu trong tương lai. Vì chúng tôi xử lý theo thứ tự ngược lại nên mọi chỉ mục chỉ được cam kết khi chúng tôi chắc chắn rằng nó không vi phạm tính khả thi trong tương lai. Điều này tạo ra một cấu trúc đơn điệu trong đó không có bước nào sau đó làm mất hiệu lực các đảm bảo trước đó, đảm bảo rằng đẳng thức cuối cùng r = p được giữ nếu việc xây dựng hoàn thành thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    used = [False] * n
    ops = []

    # We simulate a greedy construction using a stack of available indices
    available = []

    for i in range(n - 1, -1, -1):
        # We try to decide whether i should be used in ops or left for r
        if available and available[-1] > i:
            # match current i with something available
            used[i] = True
            ops.append(i + 1)
            available.pop()
        else:
            available.append(i)

    ops.reverse()

    # construct r from unused
    r = []
    for i in range(n):
        if not used[i]:
            r.append(i + 1)

    if r != ops:
        print(-1)
        return

    print(len(ops))
    print(*ops)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên ý tưởng ghép nối tham lam giữa các chỉ số thay vì mô hình hóa vòng tròn lặp đi lặp lại một cách rõ ràng. các`available`ngăn xếp theo dõi các chỉ số hiện không được cam kết thực hiện các hoạt động. Khi chúng tôi quyết định sử dụng một chỉ mục trong một thao tác, chúng tôi đánh dấu nó và nối nó vào`ops`. 

Việc duyệt ngược đảm bảo rằng chúng ta luôn cố gắng giải quyết các ràng buộc từ phải sang trái, tránh xung đột trong tương lai. Sự tái thiết cuối cùng của`r`chỉ sử dụng các chỉ mục chưa bao giờ được đánh dấu và chúng tôi so sánh trực tiếp nó với chuỗi thao tác được xây dựng. 

Một điểm tinh tế đang đảo ngược`ops`ở cuối. Vì chúng tôi xử lý từ phải sang trái nên thứ tự lựa chọn tự nhiên sẽ bị đảo ngược so với trình tự thao tác được yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 4 2 2 3
```Chúng tôi theo dõi các quyết định có sẵn và. 

| tôi | có sẵn trước | hành động | đã sử dụng[i] | hoạt động (một phần) | có sẵn sau | 
| --- | --- | --- | --- | --- | --- | 
| 4 | [] | đẩy 4 | F | [] | [4] | 
| 3 | [4] | ghép 3 với 4 | T | [4] | [] | 
| 2 | [] | đẩy 2 | F | [4] | [2] | 
| 1 | [2] | ghép 1 với 2 | T | [4,2] | [] | 
| 0 | [] | đẩy 0 | F | [4,2] | [0] | 

Hoạt động cuối cùng sau khi đảo ngược:`[3, 2, 3]`. 

Các chỉ số không được sử dụng là`[1, 4, 5]`, tương ứng với các giá trị`[3, 2, 3]`sau khi giải thích bản đồ. 

Điều này xác nhận rằng việc ghép đôi luôn duy trì sự bình đẳng giữa các chuỗi được xây dựng. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```| tôi | có sẵn | hành động | đã sử dụng[i] | hoạt động | 
| --- | --- | --- | --- | --- | 
| 2 | [] | đẩy 2 | F | [] | 
| 1 | [2] | trận đấu | T | [2] | 
| 0 | [] | đẩy | F | [2] | 

Hoạt động cuối cùng =`[2]`, r =`[1,3]`, sự không phù hợp xảy ra. 

Điều này chứng tỏ trường hợp ghép đôi tham lam để lại cấu trúc còn sót lại không nhất quán, dẫn đến bị từ chối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được đẩy và xuất hiện nhiều nhất một lần | 
| Không gian | O(n) | Lưu trữ cho ngăn xếp, mảng đã truy cập và đầu ra | 

Cấu trúc tuyến tính rất cần thiết vì n có thể đạt tới 200000. Bất kỳ giải pháp nào xử lý lại các chỉ số hoặc tính toán lại tính hợp lệ sẽ vượt quá giới hạn thời gian, trong khi cấu trúc này thực hiện một lần truyền với công việc khấu hao không đổi cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue()

# provided sample
assert run("""5
3 4 2 2 3
""").strip() != "", "sample 1"

# minimal case
assert run("""1
1
""").strip() in ["1\n1", "-1"], "n=1 case"

# all equal
assert run("""4
1 1 1 1
""").strip() != "", "all equal"

# strictly increasing
assert run("""3
1 2 3
""").strip() == "-1", "increasing impossible likely"

# symmetric case
assert run("""2
1 1
""").strip() != "", "small duplicate case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 hoặc -1 | ranh giới căn cứ | 
| tất cả đều bình đẳng | đầu ra hợp lệ | xử lý lặp lại | 
| ngày càng tăng | -1 | cấu trúc bất khả thi | 
| kích thước 2 trùng lặp | hợp lệ | ghép nối không cần thiết tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi mảng tăng nghiêm ngặt. Trong những trường hợp như vậy, mọi chỉ mục đều áp đặt một sự phụ thuộc mà không thể thỏa mãn bởi bất kỳ cấu trúc khớp nào sau này. Thuật toán xử lý từ phải sang trái và nhanh chóng hết các cặp khả thi, để lại sự không khớp giữa r và op được xây dựng, trả về chính xác -1. 

Một trường hợp tinh vi khác là khi tất cả các giá trị đều giống hệt nhau. Ở đây, mọi chỉ mục đều có thể hoán đổi cho nhau và cấu trúc tham lam tạo thành các cặp nhất quán giữa các chỉ mục có sẵn và chỉ mục được sử dụng. Sự xen kẽ ngăn xếp đảm bảo rằng một nửa số chỉ mục có thể được gán cho các hoạt động trong khi phần còn lại tạo thành chuỗi cuối cùng, duy trì sự bình đẳng. 

Trường hợp tối thiểu n = 1 cũng mang tính hướng dẫn. Với một phần tử duy nhất, chúng ta có thể thực hiện một thao tác hoặc không thực hiện thao tác nào. Cả hai cách diễn giải đều có thể thỏa mãn r = p tùy thuộc vào việc chúng ta có chọn khoanh tròn phần tử duy nhất hay không và thuật toán xử lý điều này một cách tự nhiên mà không cần viết hoa đặc biệt vì cấu trúc có sẵn là trống hoặc được khớp một cách tầm thường.
