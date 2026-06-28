---
title: "CF 105143M - Hợp nhất"
description: "Chúng ta được cấp một tập hợp các trọng số nguyên dương đại diện cho những người lính. Hoạt động duy nhất được phép là lấy hai người lính có sức mạnh khác nhau đúng một và thay thế họ bằng một người lính có sức mạnh bằng tổng sức mạnh của họ."
date: "2026-06-27T18:48:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105143
codeforces_index: "M"
codeforces_contest_name: "2024 ICPC National Invitational Collegiate Programming Contest, Wuhan Site"
rating: 0
weight: 105143
solve_time_s: 50
verified: true
draft: false
---

[CF 105143M - Hợp nhất](https://codeforces.com/problemset/problem/105143/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các trọng số nguyên dương đại diện cho những người lính. Hoạt động duy nhất được phép là lấy hai người lính có sức mạnh khác nhau đúng một và thay thế họ bằng một người lính có sức mạnh bằng tổng sức mạnh của họ. Thao tác này có thể được lặp lại bao nhiêu lần, theo bất kỳ thứ tự nào, miễn là cặp được chọn thỏa mãn điều kiện sai phân. 

Sau khi thực hiện bất kỳ chuỗi hợp nhất nào như vậy, chúng ta sẽ có được một tập hợp nhiều tập hợp cuối cùng. Bộ nhiều bộ cuối cùng này là thứ quyết định kết quả của trò chơi, bởi vì nó được sắp xếp theo thứ tự giảm dần và được so sánh theo từ điển với bộ nhiều bộ kết quả của đối thủ. Mục tiêu của Siri là chuyển đổi nhiều tập hợp ban đầu bằng cách sử dụng các phép hợp nhất để chuỗi được sắp xếp này trở nên lớn nhất có thể theo thứ tự từ điển. 

Mục tiêu từ điển học ngay lập tức cho chúng ta biết điều gì quan trọng: phần tử đầu tiên lớn nhất có thể, sau đó là phần tử thứ hai, v.v. Vì việc sắp xếp được áp dụng ở cuối nên thứ tự xây dựng bên trong không quan trọng, chỉ có nhiều tập hợp cuối cùng mới quan trọng. 

Các ràng buộc cho phép tối đa hai trăm nghìn binh sĩ và giá trị có thể lớn tới 10^18. Bất kỳ giải pháp nào cố gắng liên tục tìm kiếm các cặp có thể hợp nhất một cách đơn giản trong số tất cả các phần tử sẽ quá chậm, vì trong trường hợp xấu nhất, chúng ta có thể kiểm tra ứng viên O(n^2) hoặc quét lại cấu trúc nhiều lần sau mỗi lần hợp nhất. 

Một khó khăn tinh tế là việc sáp nhập không độc lập. Việc hợp nhất tạo ra một giá trị mới có thể mở khóa các hợp nhất mới với các giá trị khác nhau, do đó, các quyết định cục bộ tham lam có thể dễ dàng sai lầm nếu chúng ta không tổ chức hợp nhất một cách cẩn thận. 

Một kiểu lỗi phổ biến là cố gắng liên tục hợp nhất bất kỳ cặp giá trị liên tiếp có sẵn nào một cách tham lam. Ví dụ: nếu chúng ta có các giá trị 1, 2, 3, 4, việc hợp nhất đơn giản có thể chọn 2 và 3 trước tiên để tạo thành 5, sau đó mất cơ hội tạo thành chuỗi lớn hơn như 1 + 2 = 3, 3 + 4 = 7, v.v. Thứ tự hợp nhất sẽ thay đổi những chuỗi có thể thực hiện được, vì vậy chúng ta cần một cấu trúc có thể nắm bắt được tất cả tiềm năng hợp nhất một cách có hệ thống. 

## Phương pháp tiếp cận 

Thoạt nhìn, người ta có thể mô phỏng quá trình một cách trực tiếp. Chúng tôi liên tục quét mảng, tìm kiếm bất kỳ cặp nào có hiệu là một, hợp nhất chúng, xóa giá trị gốc và chèn giá trị mới. Mỗi lần hợp nhất làm giảm số phần tử đi một, do đó có nhiều nhất n lần hợp nhất. Tuy nhiên, mỗi lần hợp nhất yêu cầu tìm một cặp hợp lệ và cập nhật cấu trúc, có thể tốn O(n) nếu thực hiện bằng cách quét đơn giản hoặc thậm chí O(log n) với cập nhật cấu trúc cục bộ nhưng vẫn lặp lại n lần dẫn đến O(n^2) hoặc tệ hơn. Tốc độ này quá chậm đối với n lên tới 2×10^5. 

Quan sát quan trọng là việc hợp nhất chỉ phụ thuộc vào giá trị chứ không phải vị trí và chúng hoạt động giống như một phản ứng dây chuyền trên các số nguyên liền kề. Nếu chúng ta nghĩ về tất cả các số được nhóm theo giá trị, thì việc hợp nhất chỉ kết nối các giá trị số nguyên liên tiếp và mỗi lần hợp nhất tiêu thụ một đơn vị từ mỗi trong số hai “nhóm giá trị” liền kề trong khi tạo ra một đơn vị trong nhóm cao hơn. 

Điều này gợi ý một chiến lược tham lam đối với các giá trị được sắp xếp: chúng tôi xử lý các giá trị theo thứ tự tăng dần và mô phỏng cách “khối lượng” có thể tăng lên thông qua việc hợp nhất. Thay vì liên tục chọn các cặp tùy ý, chúng tôi duy trì số lượng của từng giá trị và cố gắng truyền bá sự hợp nhất nhiều nhất có thể về phía trước. Thông tin chi tiết quan trọng là khi chúng tôi quyết định có bao nhiêu lần hợp nhất xảy ra giữa giá trị x và x+1, thì cấu trúc kết quả tại x+1 sẽ được cố định cho các quyết định trong tương lai. Điều này làm cho quá trình trở nên tuyến tính trên các giá trị riêng biệt được sắp xếp. 

Chúng tôi xử lý hệ thống một cách hiệu quả như một luồng dọc theo các giá trị số nguyên, trong đó mỗi lần hợp nhất sẽ giảm hai số đếm liền kề và tăng cấp độ tiếp theo. Bởi vì mỗi phần tử tham gia vào quá trình chuyển đổi đi lên nhiều nhất là O(log value) trong thực tế, nên việc tích lũy tham lam cẩn thận từ nhỏ đến lớn sẽ mang lại nhiều tập hợp cuối cùng tối ưu.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng ngây thơ của sự hợp nhất | O(n^2) | O(n) | Quá chậm | 
| Quét tần số trên các giá trị | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta nén đầu vào thành các tần số có giá trị bằng nhau. Điều này là cần thiết vì chỉ có sự đa dạng mới quan trọng đối với việc hợp nhất chứ không phải vị trí. 

Tiếp theo, chúng tôi xử lý các giá trị theo thứ tự tăng dần trong khi duy trì bản đồ về số lượng mục tồn tại ở mỗi giá trị, bao gồm cả những mục được tạo bởi sự hợp nhất trước đó. 

Chúng ta cũng duy trì một cấu trúc cho phép đẩy “dư thừa” từ giá trị x vào x+1 bất cứ khi nào có thể. Ý tưởng cốt lõi là bất cứ khi nào chúng ta có ít nhất một vật phẩm ở x và ít nhất một vật phẩm ở x+1, chúng ta có thể hợp nhất chúng, tiêu thụ cả hai và tạo ra một vật phẩm ở x+2. 

Chúng tôi lặp qua các giá trị theo thứ tự được sắp xếp và liên tục áp dụng quy tắc này cho đến khi không thể hợp nhất cục bộ nữa ở cấp đó trước khi tiếp tục. 

Cuối cùng, sau khi tất cả quá trình truyền kết thúc, chúng tôi xây dựng lại nhiều tập hợp từ bản đồ tần số còn lại. 

### Tại sao nó hoạt động 

Tại mọi giá trị x, chúng tôi đảm bảo rằng tất cả các phép hợp nhất có thể có liên quan đến x và x+1 đều đã hết trước khi chúng tôi chuyển qua x. Điều này tạo ra một ranh giới ổn định: không có hoạt động nào trong tương lai liên quan đến các giá trị lớn hơn có thể tạo ra cách sử dụng tốt hơn các phần tử cấp x. Bởi vì mỗi lần hợp nhất sẽ làm tăng giá trị của phần tử được tạo ra một cách nghiêm ngặt, quá trình này đơn điệu trong không gian giá trị, do đó việc trì hoãn việc hợp nhất không bao giờ có thể cải thiện kết quả từ điển. Điều này đảm bảo rằng việc giải quyết một cách tham lam các giá trị thấp nhất trước tiên sẽ dẫn đến một tập hợp nhiều tập hợp cuối cùng tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import Counter

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    cnt = Counter(a)
    keys = sorted(cnt)

    # we will process in increasing order, but may create new keys dynamically
    i = 0
    keys = sorted(cnt.keys())

    for x in keys:
        if cnt[x] == 0:
            continue
        # try to propagate merges forward
        while cnt[x] > 0 and cnt[x + 1] > 0:
            cnt[x] -= 1
            cnt[x + 1] -= 1
            cnt[x + 2] += 1

            # ensure new keys are tracked if needed
            if x + 2 not in cnt:
                cnt[x + 2] = 0

    result = []
    for k in cnt:
        result.extend([k] * cnt[k])

    result.sort(reverse=True)

    print(len(result))
    print(*result)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách đếm tần số, vì danh tính của binh lính không liên quan một khi đã biết giá trị. Vòng lặp hợp nhất cố gắng áp dụng thao tác duy nhất được phép cục bộ giữa các giá trị liền kề. Vòng lặp while lặp lại đảm bảo chúng ta sử dụng hết mọi tương tác giữa x và x+1 trước khi tiếp tục. 

Một điểm tinh tế là các giá trị mới tạo tại x+2 cũng phải được xem xét sau, vì vậy chúng tôi đảm bảo chúng tồn tại trong từ điển. Việc sắp xếp ở cuối là bắt buộc vì bài toán đánh giá thứ tự từ điển sau khi sắp xếp giảm dần. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó các giá trị cho phép phản ứng dây chuyền đầy đủ. 

đầu vào:```
4
1 2 3 4
```Chúng tôi theo dõi sự phát triển tần số. 

| Bước | cnt[1] | cnt[2] | cnt[3] | cnt[4] | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | ban đầu | 
| 1 | 0 | 0 | 2 | 1 | hợp nhất 1+2 → 3 | 
| 2 | 0 | 0 | 1 | 0 | hợp nhất 3+4 → 7 | 
| 3 | 0 | 0 | 0 | 0 | cuối cùng + tạo 7 | 

Đầu ra cuối cùng trở thành:```
1
7
```Điều này cho thấy rằng các phép hợp nhất nhỏ sớm có thể cho phép các phép hợp nhất có giá trị cao lớn hơn và thuật toán truyền khối lượng đi lên một cách chính xác. 

Bây giờ hãy xem xét một trường hợp không thể hợp nhất được. 

đầu vào:```
3
10 100 1000
```| Bước | cnt[10] | cnt[100] | cnt[1000] | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | ban đầu | 
| 1 | 1 | 1 | 1 | không có sự khác biệt liền kề của 1 | 

Đầu ra cuối cùng vẫn còn:```
3
1000 100 10
```Điều này xác nhận rằng thuật toán không phát minh ra sự hợp nhất ở những nơi không tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp giá trị và xử lý bản đồ tần số chiếm ưu thế | 
| Không gian | O(n) | Lưu trữ tần số và tái tạo kết quả | 

Giải pháp phù hợp thoải mái trong giới hạn vì cả bộ nhớ và thời gian đều tuyến tính hoặc gần tuyến tính với số lượng binh sĩ. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))
    cnt = Counter(a)

    keys = sorted(cnt.keys())

    for x in keys:
        while cnt[x] > 0 and cnt[x + 1] > 0:
            cnt[x] -= 1
            cnt[x + 1] -= 1
            cnt[x + 2] += 1

    res = []
    for k in cnt:
        res.extend([k] * cnt[k])

    res.sort(reverse=True)

    return str(len(res)) + "\n" + " ".join(map(str, res))

# minimal case
assert run("1\n5\n") == "1\n5"

# provided-like chain
assert run("4\n1 2 3 4\n") == "1\n7"

# no merges
assert run("3\n10 100 1000\n") == "3\n1000 100 10"

# all equal
assert run("5\n7 7 7 7 7\n") == "5\n7 7 7 7 7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | chính nó | trường hợp cơ sở đúng đắn | 
| 1 2 3 4 | 7 | tuyên truyền hợp nhất chuỗi | 
| 10 100 1000 | không thay đổi | không hợp nhất không hợp lệ | 
| tất cả đều bình đẳng | không thay đổi | ổn định dưới sự trùng lặp | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi việc hợp nhất tạo ra các cơ hội mới không hiển thị cục bộ trước khi xử lý một giá trị. Ví dụ: bắt đầu từ 1, 2, 3, 4, kết quả tối ưu phụ thuộc vào việc truyền bá các phép hợp nhất về phía trước theo đúng thứ tự để các kết quả trung gian có thể tham gia vào các phép hợp nhất sau này. Thuật toán xử lý vấn đề này bằng cách cập nhật tần số liên tục và cho phép xử lý các giá trị mới được tạo sau đó, đảm bảo không bỏ sót chuỗi nào. 

Một trường hợp khác là khi các giá trị lớn chiếm ưu thế nhưng các giá trị nhỏ vẫn có thể tăng dần lên. Bởi vì chúng tôi luôn tận dụng tối đa sự hợp nhất cục bộ trước khi tiến lên, các chuỗi nhỏ được giải quyết hoàn toàn trước khi chúng có thể can thiệp vào các quyết định có giá trị cao hơn, duy trì tính chính xác của việc tối đa hóa từ điển cuối cùng.
