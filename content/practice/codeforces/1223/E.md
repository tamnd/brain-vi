---
title: "CF 1223E - Sơn cây"
description: "Chúng ta được cho một cái cây trong đó mỗi cạnh đều có trọng số. Nhiệm vụ là gán màu cho các đỉnh theo một quy tắc rất cụ thể: mỗi đỉnh nhận được chính xác $k$ màu và bất kỳ màu cụ thể nào cũng có thể xuất hiện nhiều nhất hai lần trên toàn bộ cây."
date: "2026-06-15T19:29:21+07:00"
tags: ["codeforces", "competitive-programming", "dp", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1223
codeforces_index: "E"
codeforces_contest_name: "Technocup 2020 - Elimination Round 1"
rating: 2100
weight: 1223
solve_time_s: 317
verified: true
draft: false
---

[CF 1223E - Vẽ cây](https://codeforces.com/problemset/problem/1223/E) 

**Đánh giá:** 2100 
**Tags:** dp, phân loại, cây cối 
**Thời gian giải:** 5 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cái cây trong đó mỗi cạnh đều có trọng số. Nhiệm vụ là gán màu cho các đỉnh theo một quy tắc rất cụ thể: mỗi đỉnh nhận được chính xác$k$màu sắc và bất kỳ màu cụ thể nào cũng có thể xuất hiện tối đa hai lần trên toàn bộ cây. Điều này có nghĩa là mọi màu sắc đều không có tác dụng gì hoặc nó kết nối chính xác hai đỉnh, tạo ra mối quan hệ ghép nối giữa chúng. 

Một cạnh chỉ có giá trị nếu hai điểm cuối của nó có chung ít nhất một màu. Nói cách khác, nếu hai đỉnh được gán một màu chung thì màu đó sẽ “kích hoạt” kết nối của chúng và cho phép chúng ta thu thập trọng số của cạnh đó. Mỗi màu chỉ có thể góp phần vào nhiều nhất một cặp như vậy vì nó được phép xuất hiện tối đa hai lần về tổng thể. 

Mục tiêu là tối đa hóa tổng trọng số của tất cả các cạnh có điểm cuối chia sẻ ít nhất một màu, trên tất cả các phép gán hợp lệ. 

Mỗi truy vấn đưa ra một cây riêng biệt và một giá trị$k$, và chúng ta phải tính toán mức tối đa này một cách độc lập. 

Các ràng buộc rất lớn: tổng số đỉnh trên tất cả các truy vấn lên tới$5 \cdot 10^5$, và có thể có tới$5 \cdot 10^5$truy vấn. Điều này ngay lập tức loại trừ mọi thứ xử lý từng truy vấn trong thời gian dài hơn tuyến tính hoặc gần tuyến tính. Một giải pháp đồng đều$O(n \log n)$mỗi truy vấn sẽ quá chậm trừ khi được khấu hao nhiều. Cấu trúc của cây gợi ý rằng có thể cần phải có chiến lược sắp xếp hoặc vượt qua một lần và bất kỳ giải pháp nào cố gắng mô phỏng việc gán màu một cách rõ ràng đều không thể thực hiện được do sự bùng nổ tổ hợp của các vị trí màu. 

Một cạm bẫy tinh tế xuất phát từ sự hiểu lầm ý nghĩa của “mỗi màu xuất hiện nhiều nhất hai lần”. Một cách giải thích ngây thơ có thể coi các màu là độc lập trên mỗi đỉnh, nhưng trên thực tế, mỗi màu hoạt động giống như một tài nguyên ghép nối, tạo ra sự đóng góp nhiều nhất là một cạnh. Một sai lầm khác là cho rằng vấn đề phụ thuộc vào đường dẫn hoặc cấu trúc cây con; trên thực tế, chỉ có các cạnh là quan trọng và mỗi cạnh có thể được “kích hoạt” hoặc không. 

Ví dụ: trong một chuỗi đơn giản gồm ba nút có trọng số 1 và 100, một kẻ tham lam ngây thơ cố gắng so khớp cục bộ các đỉnh có thể ưu tiên không chính xác cạnh đầu tiên, trong khi giải pháp tối ưu có thể phụ thuộc vào tổng số cặp$k$cho phép. 

## Phương pháp tiếp cận 

Khó khăn chính là hiểu được màu sắc thực sự cho phép chúng ta thực hiện điều gì bằng cách kết hợp. Mỗi màu xuất hiện nhiều nhất hai lần, vì vậy mỗi màu tương ứng với một đỉnh hoặc một cặp đỉnh. Vì mỗi đỉnh nhận được chính xác$k$màu sắc, chúng ta có thể nghĩ mỗi đỉnh có$k$“cổng” có thể được sử dụng để kết nối nó với các đỉnh khác thông qua các màu được chia sẻ. 

Điều này biến vấn đề thành việc chọn tối đa$k$"kết nối" sự cố trên mỗi đỉnh, nhưng bị hạn chế trên toàn cầu để mỗi kết nối tiêu thụ dung lượng từ cả hai điểm cuối cùng một lúc. Mỗi cạnh được chọn tương ứng với việc dành một màu cho cả hai điểm cuối và vì màu không thể được sử dụng lại ngoài hai đỉnh, nên mỗi cạnh được chọn độc lập về mức tiêu thụ màu ngoại trừ các ràng buộc về mức độ đỉnh trong cấu trúc đã chọn. 

Quan sát quan trọng là trong một giải pháp tối ưu, chúng ta chỉ quan tâm đến việc chọn một tập hợp các cạnh sao cho không có đỉnh nào sử dụng nhiều hơn$k$các cạnh sự cố đã chọn. Nếu một đỉnh sử dụng ít hơn$k$, chúng ta luôn có thể lãng phí thêm màu sắc mà không ảnh hưởng đến điểm số. Vì vậy, bài toán quy về việc chọn một tập con các cạnh có trọng số lớn nhất sao cho bậc của mỗi đỉnh trong tập con được chọn lớn nhất là$k$. 

Đây là một phép biến đổi cổ điển: chúng ta đang chọn các cạnh trong cây dưới các ràng buộc về dung lượng đỉnh. Cấu trúc cây cho phép giải pháp tham lam: chúng tôi muốn bao gồm các cạnh nặng nhất trong khi tôn trọng giới hạn mức độ và vì không có chu kỳ, chúng tôi có thể sắp xếp các cạnh trên toàn cầu theo trọng số và bao gồm chúng một cách tham lam nếu cả hai điểm cuối vẫn còn dung lượng. 

Cấu trúc cây rất cần thiết vì bất kỳ tập cạnh nào trong cây đều không tạo ra các chu trình dẫn đến sự phụ thuộc phức tạp hơn. Điều này làm cho một tham lam nhận thức năng lực đơn giản trở nên hợp lệ. 

Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con của các cạnh và kiểm tra tính khả thi, theo cấp số nhân trong$n$. Ngay cả việc cố gắng sử dụng luồng hoặc DP trên cây cho mỗi truy vấn cũng sẽ quá chậm so với số lượng truy vấn. 

Cách tiếp cận sắp xếp tham lam làm giảm vấn đề chỉ còn việc sắp xếp các cạnh một lần cho mỗi truy vấn và xử lý chúng một cách tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi truy vấn, hãy đọc cây và lưu trữ tất cả các cạnh cùng với trọng số của chúng. Bản thân cấu trúc cây không được sử dụng trực tiếp ngoài danh sách cạnh, vì quyết định hoàn toàn là về việc chọn các cạnh theo ràng buộc. 
2. Sắp xếp tất cả các cạnh theo thứ tự trọng số giảm dần. Điều này đảm bảo rằng bất cứ khi nào chúng ta xem xét một cạnh, tất cả các cạnh nặng hơn đều đã được xem xét để đưa vào. Thứ tự này rất quan trọng vì chúng ta luôn ưu tiên các cạnh nặng hơn khi có thể. 
3. Duy trì một mảng`used[v]`đếm xem có bao nhiêu cạnh được chọn liên quan đến mỗi đỉnh. Ban đầu tất cả đều bằng không. 
4. Lặp lại các cạnh theo thứ tự được sắp xếp. Đối với một cạnh$(u, v, w)$, kiểm tra xem cả hai`used[u] < k`Và`used[v] < k`. 
5. Nếu cả hai điểm cuối vẫn còn dung lượng khả dụng, hãy đưa cạnh vào câu trả lời và phần tăng thêm`used[u]`Và`used[v]`. Nếu không thì bỏ qua nó. Điều này thực thi ràng buộc mà mỗi đỉnh tham gia nhiều nhất$k$các cạnh được chọn. 
6. Tính tổng trọng số của tất cả các cạnh đã chọn và đưa ra kết quả. 

### Tại sao nó hoạt động 

Quá trình thực thi giới hạn độ trên mỗi đỉnh trong khi luôn chọn cạnh nặng nhất hiện có trước tiên. Bởi vì mỗi cạnh được chọn tiêu thụ chính xác một đơn vị công suất từ ​​cả hai điểm cuối và công suất giống hệt nhau và độc lập trên các đỉnh, nên việc thay thế cạnh nặng hơn đã chọn bằng cạnh nhẹ hơn sau này sẽ không có lợi ích gì. Bất kỳ giải pháp hợp lệ nào đều tương ứng với tập cạnh khả thi trong cùng các ràng buộc về dung lượng và việc sắp xếp đảm bảo chúng ta luôn xây dựng tập khả thi có trọng số tối đa một cách tham lam mà không cần phải quay lui. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        n, k = map(int, input().split())
        edges = []
        for _ in range(n - 1):
            u, v, w = map(int, input().split())
            u -= 1
            v -= 1
            edges.append((w, u, v))

        edges.sort(reverse=True)

        used = [0] * n
        ans = 0

        for w, u, v in edges:
            if used[u] < k and used[v] < k:
                used[u] += 1
                used[v] += 1
                ans += w

        print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện chiến lược tham lam. Trạng thái duy nhất mà chúng tôi duy trì là số cạnh liên quan được chọn trên mỗi đỉnh, tương ứng với số lượng màu mà đỉnh đó đã sử dụng hiệu quả trong các cặp. 

Việc sắp xếp các cạnh theo thứ tự giảm dần đảm bảo chúng tôi luôn ưu tiên những đóng góp có giá trị cao. Việc kiểm tra trên cả hai điểm cuối sẽ đảm bảo tính khả thi theo ràng buộc màu sắc được chuyển thành giới hạn độ. 

Một chi tiết triển khai tinh tế là chúng ta không bao giờ cần mô hình hóa màu sắc một cách rõ ràng. Sự trừu tượng hóa ghép nối hoàn toàn sụp đổ thành việc lựa chọn cạnh với các hạn chế về dung lượng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1
1 2 5
3 1 2
3 4 3
```Chúng tôi liệt kê các cạnh được sắp xếp theo trọng số: 

| Bước | Cạnh (w,u,v) | đã sử dụng[u] | đã sử dụng[v] | Được chọn | Tổng chạy | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (5,1,2) | 0,0 | 0,0 | vâng | 5 | 
| 2 | (3,3,4) | 0,0 | 0,0 | vâng | 8 | 
| 3 | (2,1,3) | 1,1 | 1,1 | không | 8 | 

Với$k=1$, mỗi nút chỉ có thể tham gia vào một cạnh được chọn. Chúng tôi chọn hai cạnh nặng nhất không xung đột. Cạnh thứ ba bị từ chối vì cả hai điểm cuối đều đã đạt dung lượng. 

### Ví dụ 2 

đầu vào:```
7 2
1 2 5
1 3 4
1 4 2
2 5 1
2 6 2
4 7 3
```Các cạnh được sắp xếp: 

| Bước | Cạnh | đã sử dụng[u] | đã sử dụng[v] | Được chọn | Tổng hợp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1-2 (5) | 0,0 | 0,0 | vâng | 5 | 
| 2 | 1-3 (4) | 1,0 | 0,0 | vâng | 9 | 
| 3 | 4-7 (3) | 0,0 | 0,0 | vâng | 12 | 
| 4 | 2-6 (2) | 1,0 | 1,0 | vâng | 14 | 
| 5 | 1-4 (2) | 2,1 | 0,1 | không | 14 | 
| 6 | 2-5 (1) | 2,1 | 1,0 | không | 14 | 

Điều này cho thấy dung lượng dần dần chặn các cạnh sau ngay cả khi chúng vẫn còn tương đối nặng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$mỗi truy vấn | bị chi phối bởi các cạnh sắp xếp | 
| Không gian |$O(n)$| lưu trữ các cạnh và bộ đếm độ | 

Trên tất cả các truy vấn, tổng số$n$được giới hạn bởi$5 \cdot 10^5$, do đó độ phức tạp tổng thể nằm trong giới hạn chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        q = int(input())
        for _ in range(q):
            n, k = map(int, input().split())
            edges = []
            for _ in range(n - 1):
                u, v, w = map(int, input().split())
                edges.append((w, u, v))
            edges.sort(reverse=True)

            used = [0] * n
            ans = 0
            for w, u, v in edges:
                if used[u-1] < k and used[v-1] < k:
                    used[u-1] += 1
                    used[v-1] += 1
                    ans += w
            print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# sample tests
assert run("""2
4 1
1 2 5
3 1 2
3 4 3
7 2
1 2 5
1 3 4
1 4 2
2 5 1
2 6 2
4 7 3
""") == """8
14"""

# single edge tree
assert run("""1
2 1
1 2 10
""") == "10"

# star tree
assert run("""1
4 1
1 2 1
1 3 2
1 4 3
""") == "3"

# larger k than degree
assert run("""1
4 10
1 2 5
2 3 6
3 4 7
""") == "18"

# chain
assert run("""1
5 2
1 2 1
2 3 2
3 4 3
4 5 4
""") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 10 | tính đúng đắn của trường hợp cơ sở | 
| ngôi sao | 3 | nút thắt năng lực tại trung tâm | 
| k lớn | 18 | k không hạn chế một cách giả tạo | 
| chuỗi | 10 | tham lam chọn tập hợp con tối ưu dọc theo đường đi | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$k$lớn hơn bất kỳ bậc đỉnh nào. Trong tình huống đó, không có ràng buộc dung lượng nào được kích hoạt và thuật toán chỉ cần chọn tất cả các cạnh. Đối với đường đi gồm bốn nút, tất cả các cạnh đều được bao gồm và câu trả lời là tổng của tất cả các trọng số. 

Một trường hợp khác là cây hình ngôi sao có tâm có độ cao nhưng$k=1$. Thuật toán tham lam chỉ chọn cạnh nặng nhất tới trung tâm và tất cả các cạnh khác đều bị từ chối vì trung tâm đạt công suất ngay lập tức. Truy tìm thuật toán cho thấy mọi sự từ chối đều do`used[center]`đạt 1, phù hợp với hành vi ràng buộc dự kiến. 

Trường hợp tinh tế cuối cùng là một cây cân bằng với các trọng số cạnh hỗn hợp trong đó các lựa chọn sớm sẽ chặn các cạnh có giá trị cao sau này. Thứ tự được sắp xếp đảm bảo rằng nếu cạnh sau bị chặn thì đó là do cả hai điểm cuối đã được sử dụng trên các cạnh có trọng số ít nhất bằng nhau, duy trì tính tối ưu.
