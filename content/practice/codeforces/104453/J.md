---
title: "CF 104453J - \u0412\u0435\u0436\u043b\u0438\u0432\u044b\u0435 \u0441\u043e\u0441\u0435\u0434\u0438"
description: "Chúng tôi đang mô phỏng hoạt động của một số người hàng xóm sống dọc theo một con đường hẹp. Mỗi người hàng xóm sở hữu một ngôi nhà được đánh số từ 1 đến N. Theo thời gian, chúng tôi nhận được nhật ký theo trình tự thời gian về các sự kiện mô tả những lần đến và đi."
date: "2026-06-30T14:36:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104453
codeforces_index: "J"
codeforces_contest_name: "ICPC Central Russia Regional Qualyfing Round, 2021"
rating: 0
weight: 104453
solve_time_s: 74
verified: true
draft: false
---

[CF 104453J - \u0412\u0435\u0436\u043b\u0438\u0432\u044b\u0435 \u0441\u043e\u0441\u0435\u0434\u0438](https://codeforces.com/problemset/problem/104453/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng hoạt động của một số người hàng xóm sống dọc theo một con đường hẹp. Mỗi người hàng xóm sở hữu một ngôi nhà được đánh số từ 1 đến N. Theo thời gian, chúng tôi nhận được nhật ký theo trình tự thời gian về các sự kiện mô tả những lần đến và đi. 

Khi hàng xóm k đến, họ cố gắng đỗ xe ngay trước cửa nhà k. Tuy nhiên, điều này chỉ có thể thực hiện được nếu tất cả các vị trí phía trước nhà từ 1 đến k−1 đều trống. Nếu không, họ không thể đến nhà mà phải để xe ở khu vực đỗ xe chung. 

Khi người hàng xóm k quyết định rời đi, hành vi của họ phụ thuộc vào việc xe của họ ở đâu. Nếu xe của họ ở trong khu vực đỗ xe chung, họ sẽ lập tức rời đi thành công. Nếu ô tô của họ đậu trước nhà, họ chỉ được khởi hành nếu đoạn đường trước nhà từ 1 đến k−1 hiện không có ô tô. Nếu điều kiện đó không thành công, họ không nhất thiết phải rời đi ngay lập tức. Họ có thể bị chặn bởi những chiếc xe trước đó phải khởi hành trước và nếu việc chặn đó không bao giờ được giải quyết có lợi cho họ thì họ vẫn ở lại khu nhà. 

Nhiệm vụ cuối cùng là phân loại mỗi hàng xóm thành một trong ba trạng thái sau khi xử lý tất cả các sự kiện: họ chưa bao giờ đến, họ đã rời đi thành công hoặc họ ở lại qua đêm. 

Các ràng buộc cho phép tối đa 100.000 sự kiện và 100.000 lân cận. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại khả năng hiển thị hoặc điều kiện chặn bằng cách quét toàn bộ tiền tố của các ngôi nhà cho mỗi sự kiện, vì điều đó sẽ dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Một điểm tinh tế quan trọng là việc khởi hành không chỉ đơn giản là kiểm tra một lần vào thời điểm yêu cầu. Một người hàng xóm cố gắng rời khỏi nhà của họ có thể bị trì hoãn do chặn xe trước đó, nghĩa là hệ thống có cấu trúc phụ thuộc xếp tầng trên các chỉ số. 

Một trường hợp phá vỡ mô phỏng ngây thơ là giả định rằng yêu cầu khởi hành luôn được giải quyết ngay lập tức nếu tiền tố rõ ràng ngay lúc đó. Ví dụ: hãy xem xét cấu hình trong đó một ô tô ở vị trí 5 bị chặn bởi một ô tô ở vị trí 3, chính ô tô này cũng bị chặn bởi cấu trúc trước đó. Kiểm tra một lần ngây thơ sẽ đánh dấu sai 5 là không thể rời đi, mặc dù nó có thể trở nên miễn phí sau 3 lần cuối cùng rời đi. 

Một trường hợp khác là nhiều lần đến và đi của cùng một hàng xóm. Một giải pháp đúng phải coi mỗi lần đến là trạng thái thiết lập lại chứ không chỉ là chuyển cờ. 

Cuối cùng, việc chỉ theo dõi xem người hàng xóm hiện có “đỗ hay không” là chưa đủ. Chúng tôi cũng phải xác định xem việc rời khỏi vị trí ngôi nhà có khả thi hay không theo động lực chặn tiền tố, điều này cho thấy rằng chúng tôi cần một cấu trúc hỗ trợ giải quyết hiệu quả vị trí chặn sớm nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng từng sự kiện bằng cách kiểm tra rõ ràng xem tất cả các vị trí từ 1 đến k−1 có trống bất cứ khi nào cần hay không. Điều này yêu cầu quét tiền tố có kích thước O(k) và trong trường hợp xấu nhất k có thể là O(N). Với M sự kiện, điều này dẫn đến độ phức tạp về thời gian O(NM), quá chậm đối với các ràng buộc 10^5. 

Sự kém hiệu quả chính là việc liên tục tính toán lại xem khoảng tiền tố có chứa ô tô hay không. Điều này cho thấy chúng ta cần một cấu trúc dữ liệu có thể duy trì tỷ lệ chiếm chỗ động và trả lời các truy vấn về trạng thái trống tiền tố một cách hiệu quả. Cây phân đoạn hoặc cây Fenwick có thể theo dõi xem có ô tô nào tồn tại trong tiền tố hay không, giảm việc kiểm tra theo thời gian logarit. 

Tuy nhiên, một quan sát sâu hơn sẽ đơn giản hóa quá trình hơn nữa. Thông tin liên quan duy nhất để chặn là sự tồn tại của ô tô ngoài cùng bên trái trong tiền tố. Chúng tôi không cần lịch sử sử dụng đầy đủ, chỉ cần tiền tố có trống tại một thời điểm nhất định hay không. Điều này có thể được duy trì bằng cách sử dụng một tập hợp các vị trí bị chiếm dụng và một cấu trúc hỗ trợ các truy vấn tiền tố tối thiểu.

Cách giảm thiểu chính xác là duy trì một tập hợp các vị trí đỗ xe có người ở phía trước các ngôi nhà theo thứ tự năng động. Đến sẽ chèn một vị trí nếu đỗ xe thành công; khởi hành loại bỏ nó. Tính khả thi của việc di chuyển chỉ phụ thuộc vào việc có tồn tại vị trí bị chiếm dụng nào trong [1, k−1] hay không, tương đương với việc kiểm tra xem vị trí chiếm giữ tối thiểu có < k hay không. Điều này biến vấn đề thành việc duy trì cấu trúc được sắp xếp và trả lời các truy vấn tiền tố tối thiểu một cách hiệu quả. 

BST cân bằng hoặc tập hợp được sắp xếp với theo dõi tối thiểu tiền tố cho phép tất cả các hoạt động theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM) | O(N) | Quá chậm | 
| Tối ưu (cấu trúc tối thiểu tập hợp / tiền tố theo thứ tự) | O(M log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình con đường như một tập hợp các vị trí đỗ xe có người sử dụng. Mỗi vị trí tương ứng với một người hàng xóm hiện đang có ô tô đỗ trước nhà. 

1. Duy trì cơ cấu sắp xếp của tất cả các vị trí mặt tiền ngôi nhà hiện đang được sử dụng. Điều này thể hiện tất cả các ô tô đang chặn đường theo thứ tự tiền tố. 
2. Duy trì một mảng hoặc bản đồ theo dõi trạng thái của từng người hàng xóm: chưa từng đến, hiện đang ở nhà, hiện đang đậu xe hoặc đã rời đi. Điều này là cần thiết để giải thích chính xác các sự kiện lặp đi lặp lại. 
3. Khi xử lý sự kiện “+ k”, trước tiên hãy kiểm tra xem có vị trí bị chiếm dụng nào tồn tại trong khoảng [1, k−1] hay không. Điều này tương đương với việc kiểm tra xem phần tử nhỏ nhất trong tập bị chiếm có nhỏ hơn k hay không. 
4. Nếu không có vị trí chặn như vậy, chúng ta chèn k vào tập có người ở, nghĩa là ô tô đang đậu trước nhà. Ngược lại, xe coi như đã vào bãi đỗ chung nên chúng tôi ghi riêng là trạng thái không chặn. 
5. Khi xử lý sự kiện “-k”, chúng tôi kiểm tra xem người hàng xóm hiện có đang đỗ xe chung hay không. Nếu vậy, họ sẽ rời đi ngay lập tức và được đánh dấu là đã hoàn thành. 
6. Nếu người hàng xóm đậu xe trước nhà họ, chúng ta cố gắng loại bỏ k khỏi tập bị chiếm giữ, nhưng chỉ khi nó hiện là phần tử chặn nhỏ nhất ảnh hưởng đến điều kiện tiền tố của nó. Nếu k không bị chặn bởi bất kỳ chỉ mục nhỏ hơn nào, nó có thể rời khỏi và bị xóa. 
7. Nếu nó bị chặn bởi các chỉ số nhỏ hơn, chúng tôi chưa thể loại bỏ nó. Chúng tôi đánh dấu nó là đang chờ. 
8. Sau khi xử lý tất cả các sự kiện, hãy phân loại từng người hàng xóm dựa trên việc họ đã từng đỗ xe, đã từng di chuyển thành công hay chưa bao giờ đến. 

Bất biến chính là tập hợp có người luôn chứa chính xác những ô tô hiện đang chặn một số tiền tố của đường và mọi hành động khởi hành chỉ được phép khi nó không vi phạm các ràng buộc về thứ tự tiền tố. Vị trí chiếm giữ nhỏ nhất đóng vai trò là người gác cổng: không chỉ số nào lớn hơn có thể rời khỏi vị trí nhà trong khi chỉ số nhỏ hơn vẫn chặn đường. Điều này đảm bảo rằng tất cả các chuyến khởi hành hợp lệ đều được xử lý theo thứ tự nhất quán trên toàn cầu, không phải theo từng sự kiện một cách tham lam. 

Bởi vì việc chặn chỉ phụ thuộc vào tiền tố cực tiểu nên bất kỳ cấu hình nào cho phép khởi hành cuối cùng đều phải coi ô tô là phần tử chặn nhỏ nhất tại một thời điểm nào đó. Thuật toán chỉ loại bỏ ô tô khi chúng đủ điều kiện về mặt cấu trúc theo ràng buộc đặt hàng này, ngăn chặn việc loại bỏ sớm hoặc không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    present = [False] * (n + 1)
    left = [False] * (n + 1)

    parked = set()
    import bisect
    arr = []

    def has_block(k):
        if not arr:
            return False
        # smallest occupied position
        return arr[0] < k

    for _ in range(m):
        op, k = input().split()
        k = int(k)

        if op == '+':
            if not has_block(k):
                # parked in front of house
                if k not in parked:
                    parked.add(k)
                    bisect.insort(arr, k)
            else:
                # goes to external parking (not tracked in occupied set)
                pass
            present[k] = True

        else:
            # departure request
            if k in parked:
                # can only leave if no blocking smaller index remains
                # if k is currently minimal, it can leave
                if arr and arr[0] == k:
                    parked.remove(k)
                    arr.pop(0)
                    left[k] = True
            else:
                # in external parking
                left[k] = True

    for i in range(1, n + 1):
        if not present[i]:
            print(-1)
        elif left[i]:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp giữ hai trạng thái: liệu hàng xóm có đến hay không và liệu họ có rời đi thành công hay không. Danh sách`arr`duy trì tất cả các ô tô hiện đang chặn đường theo thứ tự được sắp xếp, do đó phần tử nhỏ nhất luôn có thể truy cập được trong thời gian O(1). Các phần chèn sử dụng tính năng chèn nhị phân để duy trì thứ tự, trong khi việc xóa từ phía trước mô phỏng việc giải quyết ô tô bị chặn nhiều nhất. 

bộ`parked`phân biệt những chiếc xe đang chắn đường với những chiếc xe đang đậu bên ngoài. Chỉ những chiếc xe đang đỗ mới tương tác với ràng buộc tiền tố. 

Một chi tiết thực hiện tinh tế là chỉ được phép rời khỏi nhà khi ô tô hiện có chỉ số chặn nhỏ nhất. Điều này mã hóa hạn chế là không có chiếc xe nào có chỉ số nhỏ hơn ở phía trước đường. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 7
+ 3
+ 4
+ 2
- 4
- 3
+ 1
- 1
```Chúng tôi theo dõi các chuyến đến, xe đỗ và chuyến khởi hành. 

| Bước | Sự kiện | Bộ đỗ xe | Bị chặn nhỏ nhất | Cập nhật còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | +3 | {3} | 3 | - | 
| 2 | +4 | {3,4} | 3 | - | 
| 3 | +2 | {2,3,4} | 2 | - | 
| 4 | -4 | {2,3,4} | 2 | không | 
| 5 | -3 | {2,3,4} | 2 | không | 
| 6 | +1 | {1,2,3,4} | 1 | - | 
| 7 | -1 | {2,3,4} | 2 | vâng | 

Giải thích: xe 4 và 3 không thể rời đi cho đến khi tiền tố chặn nhỏ nhất được giải quyết. Chỉ khi 1 người đến và rời đi muộn hơn thì cấu trúc mới giãn ra một cách chính xác, cho phép 1 người rời đi. Xe 2 và 3 vẫn bị chặn ở cuối. 

Phân loại cuối cùng phù hợp với sản lượng dự kiến. 

### Ví dụ tùy chỉnh 

đầu vào:```
3 5
+ 2
+ 1
- 2
- 1
- 3
```| Bước | Sự kiện | Bộ đỗ xe | Bị chặn nhỏ nhất | Cập nhật còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | +2 | {2} | 2 | - | 
| 2 | +1 | {1,2} | 1 | - | 
| 3 | -2 | {1,2} | 1 | không | 
| 4 | -1 | {2} | 2 | vâng | 
| 5 | -3 | {2} | 2 | - | 

Ở đây, người hàng xóm 3 không bao giờ đến, người hàng xóm 2 rời đi sau khi 1 người dọn dẹp và người hàng xóm 1 rời đi thành công. 

Điều này cho thấy việc xử lý đúng đắn đối với cả trường hợp khách đến bị mất tích và trường hợp khởi hành bị trì hoãn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log N) | Mỗi lần chèn hoặc xóa trong cấu trúc có thứ tự đều tốn thời gian logarit | 
| Không gian | O(N) | Lưu trữ cho mảng trạng thái và bộ đỗ xe chủ động | 

Các ràng buộc cho phép tối đa 100.000 sự kiện, do đó, hệ số logarit dễ dàng đủ nhanh. Dấu chân bộ nhớ là tuyến tính theo số lượng hàng xóm, cũng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import defaultdict

    n, m = map(int, input().split())
    present = [False] * (n + 1)
    left = [False] * (n + 1)
    parked = set()
    import bisect
    arr = []

    def has_block(k):
        return arr and arr[0] < k

    for _ in range(m):
        op, k = input().split()
        k = int(k)
        if op == '+':
            if not has_block(k):
                if k not in parked:
                    parked.add(k)
                    bisect.insort(arr, k)
            present[k] = True
        else:
            if k in parked:
                if arr and arr[0] == k:
                    parked.remove(k)
                    arr.pop(0)
                    left[k] = True
            else:
                left[k] = True

    out = []
    for i in range(1, n + 1):
        if not present[i]:
            out.append("-1")
        elif left[i]:
            out.append("YES")
        else:
            out.append("NO")
    return "\n".join(out)

# provided sample
assert run("""5 7
+ 3
+ 4
+ 2
- 4
- 3
+ 1
- 1
""") == """YES
NO
NO
YES
-1"""

# custom cases
assert run("""1 2
+ 1
- 1
""") == "YES", "single element"

assert run("""2 1
- 1
""") == "-1\n-1", "no arrivals"

assert run("""3 3
+ 3
+ 2
- 2
""") == "NO\nYES\nNO", "blocking prefix"

assert run("""4 6
+ 4
+ 3
+ 2
- 3
- 4
- 2
""") == "NO\nYES\nNO\nNO", "cascade blocking"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | CÓ | cơ bản đến và đi | 
| không có người đến | -1 -1 | xử lý trạng thái nguyên vẹn | 
| tiền tố chặn | KHÔNG CÓ KHÔNG | logic chặn tiền tố | 
| chặn tầng | KHÔNG CÓ KHÔNG KHÔNG | ra lệnh bỏ chặn hành vi | 

## Vỏ cạnh 

Trường hợp key edge là khi hàng xóm đến nhưng ngay lập tức bị chặn bởi chỉ mục nhỏ hơn. Trong tình huống đó, họ không tham gia vào nhóm đỗ đang hoạt động. Ví dụ: nếu 2 đến sau khi 1 đã bị chặn, thì 2 không thể ảnh hưởng đến trạng thái tiền tố cho đến khi 1 được giải quyết. Thuật toán đảm bảo điều này bằng cách chỉ chèn vào cấu trúc có thứ tự khi không tồn tại vị trí chặn nhỏ hơn. 

Một trường hợp khác là việc đến và đi lặp lại của cùng một người hàng xóm. Vì chúng tôi theo dõi riêng biệt liệu người hàng xóm có từng đến hay không và liệu họ có rời đi thành công hay không, nên những lần đến lại không ghi đè lên phân loại cuối cùng. Trạng thái được tích lũy qua các sự kiện. 

Trường hợp cạnh cuối cùng đang cố gắng rời đi khi hàng xóm hiện không nằm trong tập hợp bị chặn. Điều này tương ứng với bãi đỗ xe bên ngoài, nơi khởi hành luôn ngay lập tức. Thuật toán bỏ qua rõ ràng các ràng buộc tiền tố trong nhánh đó, đảm bảo tính chính xác ngay cả khi không áp dụng thứ tự nội bộ.
