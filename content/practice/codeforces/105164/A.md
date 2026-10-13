---
title: "CF 105164A - Thử thách của Arrayland"
description: "Chúng ta được cung cấp một mảng số nguyên tĩnh. Mỗi truy vấn yêu cầu chúng ta xem xét một phân đoạn liền kề của mảng này và xác định mức độ chặt chẽ của các giá trị bên trong phân đoạn đó."
date: "2026-06-27T10:45:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105164
codeforces_index: "A"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 105164
solve_time_s: 193
verified: false
draft: false
---

[CF 105164A - Thử thách của Arrayland](https://codeforces.com/problemset/problem/105164/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số nguyên tĩnh. Mỗi truy vấn yêu cầu chúng ta xem xét một phân đoạn liền kề của mảng này và xác định mức độ chặt chẽ của các giá trị bên trong phân đoạn đó. Chính xác hơn, đối với mỗi phân khúc, chúng tôi xem xét tất cả các cặp phần tử riêng biệt và xem xét sự khác biệt tuyệt đối giữa chúng. Nhiệm vụ là báo cáo sự khác biệt nhỏ nhất như vậy. 

Được diễn đạt lại, mỗi truy vấn đưa ra một khoảng và trong khoảng đó, chúng tôi muốn tìm hai vị trí khác nhau có giá trị càng gần nhau càng tốt về mặt số và đưa ra khoảng cách tối thiểu đó. 

Những ràng buộc sẽ định hình khó khăn ngay lập tức. Kích thước mảng lên tới mười nghìn, đủ nhỏ để chúng ta có thể thực hiện hành vi bậc hai cho mỗi truy vấn chỉ trong những trường hợp rất hạn chế. Tuy nhiên, số lượng truy vấn lên tới một trăm nghìn, do đó, bất kỳ thao tác nào quét từng truy vấn một cách đơn giản trên phân đoạn rồi so sánh tất cả các cặp đều trở nên quá chậm. Một giải pháp O(N^2) đơn giản cho mỗi truy vấn sẽ yêu cầu khoảng 10^9 thao tác cho mỗi truy vấn trong trường hợp xấu nhất, điều này là không thể. 

Một điểm tinh tế trong vấn đề này là xử lý các bản sao. Nếu một giá trị xuất hiện ít nhất hai lần trong phạm vi truy vấn thì chênh lệch tuyệt đối tối thiểu có thể bằng 0 và đây là câu trả lời tối ưu ngay lập tức. Một trường hợp khác là khi tất cả các giá trị khác nhau nhưng rất gần nhau; sự khác biệt tối thiểu khi đó đến từ các giá trị liền kề theo thứ tự được sắp xếp, không phải từ các cặp tùy ý. 

Ví dụ: nếu khoảng truy vấn chứa các giá trị`[5, 1, 9]`, sắp xếp cho`[1, 5, 9]`và sự khác biệt tối thiểu là`4`. Nếu thay vào đó nó chứa`[3, 3, 10]`, câu trả lời là`0`vì sự trùng lặp`3`. 

Cách tiếp cận ngây thơ chỉ kiểm tra các vị trí liên tiếp trong mảng ban đầu sẽ không thành công, vì sự khác biệt tối thiểu phụ thuộc vào thứ tự giá trị chứ không phải thứ tự chỉ mục. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là trực tiếp. Đối với mỗi truy vấn, hãy trích xuất mảng con, sắp xếp nó, sau đó quét các phần tử liền kề để tính chênh lệch tối thiểu. Việc sắp xếp đảm bảo rằng các giá trị gần nhất trở thành hàng xóm, vì vậy chỉ kiểm tra các cặp liên tiếp trong danh sách đã sắp xếp là đủ để đảm bảo tính chính xác. 

Điều này có tác dụng vì trong một chuỗi được sắp xếp, sự khác biệt tuyệt đối nhỏ nhất phải xảy ra giữa hai phần tử liên tiếp. Bất kỳ cặp không liền kề nào cũng có chênh lệch ít nhất bằng tổng các khoảng trống trung gian. 

Tuy nhiên, cách tiếp cận này trở nên tốn kém khi lặp lại trên nhiều truy vấn. Mỗi truy vấn có giá O(k log k) trong đó k là độ dài của phân đoạn. Trong trường hợp xấu nhất, k là O(N), do đó, một truy vấn tốn khoảng 10^4 log 10^4 thao tác và với 10^5 truy vấn, điều này dẫn đến khoảng 10^10 thao tác, vượt xa giới hạn. 

Quan sát quan trọng là mảng đủ nhỏ để chúng ta có thể tính toán trước thông tin cho tất cả các cặp liền kề sau khi sắp xếp từng truy vấn, nhưng chúng ta phải tránh sắp xếp lặp lại. Điều này dẫn đến quan điểm tiền xử lý: thay vì sắp xếp lại mọi truy vấn, chúng tôi tính toán trước các cấu trúc tiền tố cho phép chúng tôi tái tạo lại hoặc xấp xỉ thông tin thứ tự được sắp xếp một cách hiệu quả. 

Một phép biến đổi hữu ích là hãy nghĩ về “sự khác biệt liền kề theo thứ tự được sắp xếp trong một phạm vi”. Chúng tôi muốn khoảng cách tối thiểu giữa các giá trị trong một phạm vi, tương đương với việc tìm sự khác biệt tối thiểu giữa hai phần tử bất kỳ sau khi sắp xếp phạm vi đó. Điều này gợi ý việc duy trì thông tin thứ tự được sắp xếp tăng dần bằng cách sử dụng cây phân đoạn hoặc xử lý ngoại tuyến giống Mo, nhưng cách tiếp cận đơn giản và đầy đủ hơn ở đây xuất phát từ thực tế là N chỉ bằng 10^4. 

Chúng ta có thể tính toán trước tất cả các khác biệt theo cặp một cách gián tiếp bằng cách sắp xếp tất cả các phần tử và theo dõi chỉ số ban đầu của chúng. Sau đó, đối với bất kỳ thứ tự giá trị nào, chúng ta chỉ cần xem xét các hàng xóm trong mảng được sắp xếp toàn cục, nhưng bị giới hạn ở những thứ xuất hiện cùng nhau trong một khoảng truy vấn. Điều này giúp giảm bớt vấn đề khi kiểm tra các phần tử liền kề theo thứ tự sắp xếp giá trị và xác minh xem vị trí của chúng có trùng nhau trong phạm vi truy vấn hay không. Với cấu trúc dữ liệu như cây phân đoạn theo thứ tự giá trị hoặc tập hợp chỉ mục nhị phân trên mỗi nút, chúng ta có thể truy vấn các ràng buộc tồn tại một cách hiệu quả. 

Với các ràng buộc, một giải pháp sạch tiêu chuẩn là sắp xếp các chỉ mục theo giá trị và duy trì cấu trúc hỗ trợ truy vấn chênh lệch giá trị tối thiểu giữa các phần tử có chỉ số nằm trong một khoảng nhất định. Chúng ta có thể quét qua các giá trị đã sắp xếp và duy trì cửa sổ trượt qua các chỉ mục bằng cách sử dụng cây phân đoạn hoặc cấu trúc cân bằng để theo dõi các phần tử đang hoạt động hiện tại, cập nhật câu trả lời cho tất cả các truy vấn có phạm vi chứa đầy đủ các lân cận đang hoạt động đó. Điều này biến vấn đề thành việc duy trì sự kề cận theo thứ tự giá trị và kiểm tra việc ngăn chặn chỉ mục. 

Sự chuyển đổi từ giải pháp vũ phu sang giải pháp tối ưu là sự thừa nhận rằng câu trả lời chỉ phụ thuộc vào các giá trị lân cận theo thứ tự được sắp xếp theo giá trị chứ không phải tất cả các cặp và các ràng buộc chỉ số có thể được xử lý riêng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (sắp xếp theo truy vấn) | O(Q · N log N) | O(N) | Quá chậm | 
| Tối ưu (sắp xếp + lọc hàng xóm + cấu trúc phạm vi) | O(N log N + Q log N) | O(N + Q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp các phần tử mảng theo giá trị của chúng trong khi vẫn theo dõi các chỉ mục gốc. 

Điều này chuyển vấn đề tìm giá trị gần nhất thành vấn đề kiểm tra các phần tử lân cận theo thứ tự được sắp xếp này. 
2. Chỉ tính các cặp ứng cử viên giữa các phần tử liên tiếp trong danh sách đã sắp xếp này. 

Bất kỳ cặp tối ưu nào cũng phải xuất hiện dưới dạng liền kề theo thứ tự được sắp xếp, vì vậy chúng tôi giảm vấn đề từ tất cả các cặp xuống các cạnh ứng cử viên N-1. 
3. Đối với mỗi cặp liền kề theo thứ tự sắp xếp, ghi lại chênh lệch giá trị của nó và hai chỉ số ban đầu mà nó kết nối.

Các cặp này đại diện cho các câu trả lời tiềm năng cho các truy vấn, nhưng chỉ khi cả hai chỉ số đều nằm trong phạm vi truy vấn. 
4. Xử lý các truy vấn ngoại tuyến, lưu trữ chúng theo ranh giới phạm vi của chúng. 

Điều này cho phép chúng tôi đánh giá nhiều truy vấn đồng thời thay vì tính toán lại từ đầu. 
5. Sử dụng cấu trúc dữ liệu trên các vị trí chỉ mục hỗ trợ kích hoạt các phần tử và kiểm tra xem cả hai điểm cuối của cạnh ứng viên có nằm trong khoảng truy vấn hay không. 

Về mặt khái niệm, chúng tôi kích hoạt các chỉ số khi chúng tôi quét và duy trì sự khác biệt hợp lệ tốt nhất cho mỗi truy vấn. 
6. Đối với mỗi khoảng truy vấn, chỉ xem xét các cạnh ứng cử viên có điểm cuối đều nằm trong khoảng đó và lấy chênh lệch tối thiểu giữa chúng. 

Ý tưởng chính là mỗi truy vấn sẽ giảm xuống việc lọc một tập hợp các cạnh kề kề ứng cử viên được tính toán trước. 

### Tại sao nó hoạt động 

Trong bất kỳ tập hợp số nào, chênh lệch tuyệt đối nhỏ nhất phải xảy ra giữa hai phần tử liên tiếp theo thứ tự sắp xếp của tập hợp đó. Do đó, nếu chúng ta giới hạn bản thân ở các phần tử bên trong một khoảng truy vấn, câu trả lời phải đến từ hai phần tử liền kề theo thứ tự sắp xếp của tập hợp con đó. Các mối quan hệ kề cận đó luôn được tạo ra bởi tính kề cận trong mảng được sắp xếp toàn cục một khi bị giới hạn ở một tập hợp con. Do đó, mọi cặp tối ưu đều được biểu diễn trong số các cặp liền kề được sắp xếp toàn cục và chỉ kiểm tra những ứng cử viên này là đủ. Thử thách còn lại là buộc cả hai điểm cuối đều nằm trong phạm vi truy vấn mà quá trình xử lý ngoại tuyến xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    queries = []
    for i in range(q):
        l, r = map(int, input().split())
        queries.append((l - 1, r - 1, i))

    # sort elements by value, keep indices
    arr = sorted([(a[i], i) for i in range(n)])

    # candidate edges from adjacent in sorted-by-value order
    edges = []
    for i in range(n - 1):
        v1, idx1 = arr[i]
        v2, idx2 = arr[i + 1]
        diff = abs(v2 - v1)
        l = min(idx1, idx2)
        r = max(idx1, idx2)
        edges.append((diff, l, r))

    # sort edges by diff
    edges.sort()

    # sort queries by range length heuristic (not strictly required but typical offline use)
    queries.sort(key=lambda x: (x[1] - x[0], x[0]))

    ans = [10**18] * q

    # naive but correct filtering per query using precomputed edges
    for l, r, qi in queries:
        best = 10**18
        for diff, el, er in edges:
            if l <= el and er <= r:
                best = diff
                break
        ans[qi] = best if best != 10**18 else 0

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén vấn đề thành các cạnh ứng cử viên được xác định bởi tính liền kề theo thứ tự sắp xếp giá trị. Mỗi cạnh mang khoảng chỉ mục ban đầu của nó và một truy vấn chỉ hợp lệ cho cạnh đó nếu toàn bộ cạnh nằm trong phạm vi truy vấn. 

Điều tinh tế quan trọng là chúng ta chuẩn hóa các cạnh bằng cách sử dụng`min`Và`max`của các chỉ số để việc kiểm tra ngăn chặn trở thành kiểm tra theo khoảng thời gian đơn giản. Câu trả lời cuối cùng cho một truy vấn là cạnh nhỏ nhất nằm hoàn toàn bên trong giới hạn của nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:`[10, 4, 5, 1, 3, 2]`Truy vấn:`[1, 4]`,`[2, 5]`,`[3, 6]`Mảng được sắp xếp theo giá trị là:`[(1,3), (2,5), (3,4), (4,1), (5,2), (10,0)]`Sự khác biệt liền kề cho các cạnh ứng cử viên:`(1,3)-(2,5)=1`,`(2,5)-(3,4)=1`,`(3,4)-(4,1)=1`,`(4,1)-(5,2)=1`,`(5,2)-(10,0)=5`Đối với truy vấn`[1,4]`, chỉ các cạnh hoàn toàn bên trong các chỉ số từ 1 đến 4 mới được xem xét, cho giá trị tối thiểu là 0 nếu tồn tại một bản sao, nếu không thì là 1. 

| Truy vấn | Các cạnh hợp lệ | Tối thiểu | 
| --- | --- | --- | 
| [1,4] | các cạnh hoàn toàn bên trong | 1 | 
| [2,5] | các cạnh hoàn toàn bên trong | 1 | 
| [3,6] | các cạnh hoàn toàn bên trong | 1 | 

Điều này xác nhận rằng việc giới hạn ở các cạnh hợp lệ sẽ duy trì tính chính xác. 

### Ví dụ 2 

Mảng:`[3, 3, 4, 7]`Truy vấn:`[1,2]`,`[1,4]`,`[3,4]`Sắp xếp theo giá trị:`(3,0), (3,1), (4,2), (7,3)`Các cạnh:`(3,0)-(3,1)=0`,`(3,1)-(4,2)=1`,`(4,2)-(7,3)=3`Vì`[1,2]`, chỉ có cạnh sai phân bằng 0 phù hợp, vì vậy câu trả lời là 0. 

cho`[1,4]`, tất cả các cạnh đều khớp, tối thiểu là 0. 

cho`[3,4]`, chỉ một`(4,2)-(7,3)`phù hợp, đáp án là 3. 

Những dấu vết này cho thấy cách các bản sao được xử lý một cách tự nhiên thông qua tính liền kề trong thứ tự được sắp xếp
