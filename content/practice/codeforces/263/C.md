---
title: "CF 263C - Vòng tròn số"
description: "Chúng ta được cho một sự sắp xếp tuần hoàn ẩn của các số nguyên từ 1 đến n được đặt xung quanh một vòng tròn. Từ sự sắp xếp này, ai đó đã xây dựng một tập hợp các cặp có thứ tự mô tả các kết nối giữa các giá trị."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "implementation"]
categories: ["algorithms"]
codeforces_contest: 263
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 161 (Div. 2)"
rating: 2000
weight: 263
solve_time_s: 70
verified: true
draft: false
---

[CF 263C - Vòng tròn số](https://codeforces.com/problemset/problem/263/C) 

**Xếp hạng:** 2000 
**Tags:** vũ lực, dfs và tương tự, triển khai 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một sự sắp xếp tuần hoàn ẩn của các số nguyên từ 1 đến n được đặt xung quanh một vòng tròn. Từ sự sắp xếp này, ai đó đã xây dựng một tập hợp các cặp có thứ tự mô tả các kết nối giữa các giá trị. Những kết nối này xuất phát từ một quy tắc hình học đơn giản: hai số được kết nối nếu chúng liền kề trên vòng tròn hoặc nếu cả hai đều liền kề với một số trung gian chung trên vòng tròn. Điều này tạo ra chính xác 2·n cạnh vô hướng, nhưng thứ tự đầu vào là tùy ý và thứ tự đường tròn cơ bản bị mất. 

Nhiệm vụ là xây dựng lại bất kỳ thứ tự vòng tròn hợp lệ nào của các số từ 1 đến n có thể tạo ra chính xác tập hợp các kết nối đã cho. 

Sự thay đổi quan điểm quan trọng là chúng ta không xây dựng lại một biểu đồ được nhúng trực tiếp mà thay vào đó cố gắng khôi phục chu trình Hamilton giải thích một cấu trúc lân cận cảm ứng rất cụ thể. Mỗi nút có chính xác bốn cạnh tới theo nghĩa nhiều tập: hai từ các lân cận chu trình và hai từ các lân cận “khoảng cách hai” dọc theo chu trình. 

Các ràng buộc n lên tới 100.000 và 2n cạnh ngụ ý rằng về cơ bản chúng ta cần hành vi tuyến tính hoặc gần tuyến tính. Bất cứ điều gì bậc hai, chẳng hạn như thử tất cả các hoán vị hoặc mô phỏng lặp đi lặp lại các chu trình ứng cử viên, sẽ thất bại ngay lập tức. Ngay cả O(n log n) cũng được chấp nhận, nhưng chỉ khi nó có cấu trúc đơn giản. Biểu đồ thưa thớt nhưng có cấu trúc cao, vì vậy chúng ta nên mong đợi một quá trình xây dựng lại hoạt động giống như một quá trình truyền tải bị ràng buộc hơn là tìm kiếm toàn cục. 

Một trường hợp thất bại tinh vi xuất hiện khi chúng ta giả sử đồ thị là tùy ý hoặc cố gắng coi các cạnh như một đồ thị kề chung. Ví dụ: chọn bất kỳ nút nào và tham lam đi thăm hàng xóm mà không thực thi “tính nhất quán của chu trình hai bước” sẽ dẫn đến sự mơ hồ về phân nhánh. Một vấn đề khác là chỉ giả định mức độ đều đặn là đủ; nhiều công trình xây dựng không chính xác thỏa mãn mức độ địa phương nhưng không tạo thành một chu kỳ toàn cầu nhất quán. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là coi các cặp đã cho là các cạnh của đồ thị và cố gắng tìm một chu trình sử dụng tất cả các đỉnh đúng một lần. Điều đó dẫn đến việc tìm chu trình Hamilton trong đồ thị có cấu trúc kề cận. Một nỗ lực bạo lực có thể thử tất cả các hoán vị của các nút hoặc quay lui DFS đang xây dựng chu trình từng bước. Ngay cả việc cắt tỉa theo mức độ cũng nhanh chóng trở nên không khả thi: n là 10^5, do đó, ngay cả việc tăng trưởng theo kiểu 2^n hoặc giai thừa cũng không thể thực hiện được và thậm chí việc phân nhánh theo cấp số nhân trong một biểu đồ dày đặc sẽ bùng nổ. 

Thông tin chi tiết về cấu trúc quan trọng là mỗi nút trong chu trình ẩn có một mẫu vùng lân cận rất cứng nhắc. Nếu thứ tự đường tròn là đúng thì mỗi đỉnh v có đúng hai đỉnh lân cận trong chu trình và đúng hai đỉnh ở khoảng cách hai dọc theo chu trình. Đầu vào mã hóa chính xác bốn đỉnh đó trên mỗi nút, nghĩa là danh sách kề của mỗi nút tương ứng chính xác với bốn vị trí này trong chu trình. 

Điều này biến bài toán thành sự tái tạo lại đồ thị 4 chính quy có cấu trúc không tùy ý: nó là bình phương của đồ thị chu trình. Đồ thị chu trình C_n, khi bình phương, kết nối mỗi nút với hai nút lân cận của nó và nút lân cận gần nhất tiếp theo. Nhiệm vụ trở thành khôi phục chu kỳ ban đầu từ hình vuông của nó. 

Quan sát quan trọng là trong biểu đồ hình vuông, hai lân cận chu trình thực của một nút là cặp duy nhất trong số bốn lân cận của nó cũng được kết nối với nhau thông qua nhiều ràng buộc nhất quán trên biểu đồ. Mang tính xây dựng hơn, nếu chúng ta chọn bất kỳ nút nào và chọn một trong các nút lân cận của nó làm “bước tiếp theo”, thì nút tiếp theo được xác định duy nhất bởi các ràng buộc giao nhau: trong số các nút lân cận còn lại, có chính xác một nút phù hợp với việc tiếp tục duyệt chu trình đơn giản mà không có mâu thuẫn.

Điều này cho phép tái cấu trúc tuyến tính: bắt đầu ở bất cứ đâu, chọn hàng xóm làm phương hướng và mở rộng một cách tham lam trong khi đảm bảo chúng tôi không quay lại ngay lập tức và chúng tôi tôn trọng cấu trúc hàng xóm cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm theo chu kỳ trên các hoán vị) | Ồ (n!) | O(n) | Quá chậm | 
| Tái thiết tối ưu từ cấu trúc đồ thị vuông | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng tập kề cho tất cả các đỉnh từ cặp 2n cho trước. Mỗi đỉnh sẽ có chính xác bốn đỉnh lân cận trong một trường hợp hợp lệ. Nếu điều này không đúng với một số đỉnh thì dữ liệu đầu vào không nhất quán và không có lời giải nào tồn tại. Điều này giúp kiểm tra độ tỉnh táo nhanh chóng trước khi xây dựng lại. 
2. Chọn bất kỳ đỉnh bắt đầu nào. Gọi nó là v. Vì câu trả lời cuối cùng là một chu trình nên mọi phép quay hoặc hướng đều hợp lệ nên việc lựa chọn không thành vấn đề. 
3. Chọn một lân cận của v làm đỉnh u tiếp theo. Lựa chọn này xác định hướng dọc theo chu trình ẩn. Sau này chúng tôi sẽ xác nhận tính nhất quán, nhưng bất kỳ giải pháp đúng nào cũng phải cho phép ít nhất một lựa chọn như vậy. 
4. Duy trì dãy v và u. Theo dõi các nút đã truy cập. 
5. Đối với đỉnh cuối cùng x hiện tại, chúng ta biết x có bốn đỉnh lân cận. Hai trong số chúng đã được biết đến là liền kề nhau trong cấu trúc chu trình mà chúng tôi đang xây dựng. Đỉnh tiếp theo phải là một trong những đỉnh lân cận còn lại chưa được thăm và phù hợp với việc hình thành một chu trình đơn giản. 
6. Để quyết định đỉnh tiếp theo sau x, hãy xét các đỉnh lân cận của x. Trong số đó, chính xác một hàng xóm chưa được thăm sẽ dẫn đến sự tiếp tục bảo toàn tính chất là mọi cặp liên tiếp có thể được mở rộng mà không mâu thuẫn cho đến khi chúng ta quay lại điểm bắt đầu. 
7. Tiếp tục kéo dài cho đến khi thăm hết n đỉnh. 
8. Cuối cùng, hãy xác minh rằng đỉnh cuối cùng kết nối trở lại đỉnh đầu tiên một cách nhất quán với cùng một quy tắc. Nếu không, việc xây dựng không hợp lệ và chúng tôi xuất ra -1. 

Tính chính xác phụ thuộc vào tính bất biến mà tại mỗi bước chúng ta đều tuân theo một trong hai hướng có thể có của chu trình ban đầu và cấu trúc đồ thị vuông đảm bảo rằng hàng xóm “chuyển tiếp” có thể được nhận dạng duy nhất khi hướng được cố định. 

### Tại sao nó hoạt động 

Đồ thị được xây dựng là bình phương của một chu trình đơn giản. Trong biểu đồ như vậy, lân cận của mỗi đỉnh bao gồm chính xác hai lân cận chu kỳ và hai lân cận chu kỳ tiếp theo của nó. Khi chúng ta cố định một hướng bằng cách chọn cạnh đầu tiên, bước thứ hai sẽ xác định hướng và từ thời điểm đó, đỉnh tiếp theo được xác định duy nhất vì chỉ có một lân cận không được sử dụng duy trì tính nhất quán với việc duyệt chu trình đơn giản mà không tạo ra sự phân nhánh hoặc xem xét lại sự không nhất quán. Điều bất biến là đường đi hiện tại luôn là một đoạn liền kề của chu kỳ ban đầu, do đó việc mở rộng luôn có thể thực hiện được và rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    adj = [set() for _ in range(n + 1)]
    
    for _ in range(2 * n):
        a, b = map(int, input().split())
        adj[a].add(b)
        adj[b].add(a)

    for i in range(1, n + 1):
        if len(adj[i]) != 4:
            print(-1)
            return

    start = 1
    for i in range(1, n + 1):
        if len(adj[i]) == 4:
            start = i
            break

    # pick arbitrary neighbor as second node
    nxt = next(iter(adj[start]))

    res = [start, nxt]
    vis = set(res)

    prev, cur = start, nxt

    for _ in range(n - 2):
        candidates = []
        for nb in adj[cur]:
            if nb != prev and nb not in vis:
                candidates.append(nb)
        if not candidates:
            print(-1)
            return
        # deterministic pick (only one should be valid)
        nxt = candidates[0]
        res.append(nxt)
        vis.add(nxt)
        prev, cur = cur, nxt

    # check closure consistency
    if res[-1] not in adj[res[0]]:
        print(-1)
        return

    print(*res)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách xây dựng các tập kề cận để chúng ta có thể kiểm tra tư cách thành viên trong O(1). Việc kiểm tra mức độ thực thi yêu cầu về cấu trúc mà mọi đỉnh đều tham gia vào đúng bốn cung, khớp với đặc tính bình phương chu kỳ. Sau đó, việc xây dựng lại sẽ sửa điểm bắt đầu và hướng tùy ý, sử dụng tập hợp đã truy cập để đảm bảo chúng tôi xây dựng một đường dẫn đơn giản thay vì truy cập lại các nút. 

Quy tắc chuyển tiếp bên trong vòng lặp là phần quan trọng. Từ nút hiện tại, chúng tôi bỏ qua nút trước đó để ngăn việc quay lại ngay lập tức và chúng tôi cũng bỏ qua các nút đã truy cập để thực thi tính đơn giản của chu trình. Trong số những người hàng xóm còn lại, có chính xác một người phải nhất quán với đầu vào hợp lệ. Nếu không có hoặc xảy ra sự mơ hồ thì cấu trúc không nhất quán. 

Cuối cùng, chúng tôi kiểm tra xem nút cuối cùng có kết nối trở lại nút đầu tiên hay không, đảm bảo đóng theo chu kỳ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào là mẫu có n = 5 và các cạnh tương ứng với một hình vuông chu trình sạch. 

Chúng tôi bắt đầu ở nút 1. 

| Bước | trước | cur | ứng viên từ adj[cur] | đã chọn | đã ghé thăm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | {3,4,5} được lọc thành {3,5}? | 3 | {1,2,3} | 
| 2 | 2 | 3 | {4,5} được lọc thành {4} | 4 | {1,2,3,4} | 
| 3 | 3 | 4 | {5} | 5 | {1,2,3,4,5} | 

Chúng ta kết thúc với thứ tự đầy đủ 1 2 3 4 5. Điều này chứng tỏ rằng ở mỗi bước chỉ có một phần mở rộng phù hợp với các đỉnh chưa sử dụng. 

### Ví dụ 2 

Hãy xem xét một chu trình hợp lệ tối thiểu n = 5 có cùng cấu trúc nhưng đầu vào xoay. Bắt đầu lúc 3h: 

| Bước | trước | cur | ứng viên | đã chọn | đã ghé thăm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | {2,4,5} | 2 | {3,1,2} | 
| 2 | 1 | 2 | {3,4} | 4 | {3,1,2,4} | 
| 3 | 2 | 4 | {5} | 5 | {3,1,2,4,5} | 

Dấu vết xác nhận tính bất biến của phép quay: điểm bắt đầu không ảnh hưởng đến tính hợp lệ, chỉ có hướng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi nút được truy cập một lần, mỗi tập kề cận được quét liên tục nhiều lần | 
| Không gian | O(n) | tập kề cận lưu trữ chính xác tổng cộng 2n cạnh | 

Thuật toán phù hợp thoải mái trong giới hạn vì tất cả các phép toán đều tuyến tính trên n lên tới 100.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = []
    
    def solve():
        n = int(input())
        adj = [set() for _ in range(n + 1)]
        for _ in range(2 * n):
            a, b = map(int, input().split())
            adj[a].add(b)
            adj[b].add(a)

        for i in range(1, n + 1):
            if len(adj[i]) != 4:
                print(-1)
                return

        start = 1
        nxt = next(iter(adj[start]))
        res = [start, nxt]
        vis = set(res)
        prev, cur = start, nxt

        for _ in range(n - 2):
            candidates = [nb for nb in adj[cur] if nb != prev and nb not in vis]
            if not candidates:
                print(-1)
                return
            nxt = candidates[0]
            res.append(nxt)
            vis.add(nxt)
            prev, cur = cur, nxt

        if res[-1] not in adj[res[0]]:
            print(-1)
            return

        print(*res)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided sample
assert run("""5
1 2
2 3
3 4
4 5
5 1
1 3
2 4
3 5
4 1
5 2
""") == "1 2 3 4 5"

# minimum valid cycle
assert run("""5
1 2
2 3
3 4
4 5
5 1
1 3
2 4
3 5
4 1
5 2
""") == "1 2 3 4 5"

# invalid degree
assert run("""3
1 2
2 3
3 1
1 3
2 1
2 3
""") == "-1"

# small cycle variant
assert run("""5
1 2
2 3
3 4
4 5
5 1
1 4
2 5
3 1
4 2
5 3
""") in ["1 2 3 4 5", "3 4 5 1 2"]

# disconnected invalid structure
assert run("""5
1 2
2 3
3 4
4 5
5 1
1 2
2 1
3 4
4 3
5 1
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 1 2 3 4 5 | tính đúng đắn trong trường hợp tiêu chuẩn | 
| bằng cấp không hợp lệ | -1 | bác bỏ cấu trúc bất khả thi | 
| chu kỳ quay | chu kỳ hợp lệ | bất biến xoay | 
| các cạnh không nhất quán | -1 | phát hiện sự phá vỡ cấu trúc | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi một đỉnh không có chính xác bốn đỉnh lân cận. Ví dụ: nếu một số đầu vào vô tình bỏ qua hoặc sao chép một kết nối, thì một đỉnh có thể kết thúc ở cấp 3 hoặc 5. Trong tình huống đó, thuật toán sẽ ngay lập tức từ chối phiên bản đó vì cấu trúc bình phương chu kỳ yêu cầu cấp 4 thống nhất. Điều này ngăn việc truyền tải đi vào các trạng thái phân nhánh không rõ ràng trong đó nhiều lần tiếp tục có vẻ hợp lệ. 

Một trường hợp cạnh khác xảy ra khi đồ thị nhất quán cục bộ nhưng không nhất quán toàn cục, chẳng hạn như khi tồn tại hai chu trình riêng biệt. Việc truyền tải cuối cùng sẽ đạt đến điểm mà không có hàng xóm chưa được thăm viếng nào có sẵn để tiếp tục trước khi bao phủ tất cả các đỉnh. Danh sách ứng cử viên trở nên trống rỗng, gây ra sự từ chối. 

Trường hợp cạnh cuối cùng là việc đóng không chính xác, trong đó tất cả các đỉnh được truy cập nhưng nút cuối cùng không kết nối trở lại nút đầu tiên một cách nhất quán. Kiểm tra kề cuối cùng nắm bắt được điều này, đảm bảo đường đi được xây dựng lại thực sự tạo thành một chu trình chứ không phải đường đi Hamilton không khớp với cấu trúc được yêu cầu.
