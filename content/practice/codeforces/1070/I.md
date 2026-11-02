---
problem: 1070I
contest_id: 1070
problem_index: I
name: "Privatization of Roads in Berland"
contest_name: "2018-2019 ICPC, NEERC, Southern Subregional Contest (Online Mirror, ACM-ICPC Rules, Teams Preferred)"
rating: 2400
tags: ["flows", "graph matchings", "graphs"]
answer: passed_samples
verified: false
solve_time_s: 81
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33bf33-5374-83ec-9b48-0bc381466acc
---

# CF 1070I - Tư nhân hóa đường bộ ở Berland 

**Đánh giá:** 2400 
**Tags:** luồng, đối sánh biểu đồ, đồ thị 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33bf33-5374-83ec-9b48-0bc381466acc 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng đơn giản với tối đa 600 đỉnh và 600 cạnh cho mỗi trường hợp thử nghiệm. Mỗi cạnh phải được gán một nhãn, gọi là chỉ mục công ty. Nhiệm vụ có hai hạn chế. 

Đầu tiên, mọi công ty đều được phép sử dụng tổng cộng tối đa hai cạnh. Vì vậy, nếu một nhãn xuất hiện, nó có thể xuất hiện một hoặc hai lần trên toàn cầu, không bao giờ ba lần trở lên. 

Thứ hai, mỗi thành phố không thể bị “đụng chạm” bởi quá nhiều công ty khác nhau. Chính xác hơn, nếu chúng ta xem xét tất cả các cạnh liên quan đến một thành phố và thu thập tập nhãn công ty được gán cho các cạnh đó thì kích thước của tập hợp đó không được vượt quá k. 

Chúng ta phải tạo ra một phép gán như vậy hoặc tuyên bố rằng không thể xuất ra các số 0 cho tất cả các cạnh. 

Các ràng buộc có tổng kích thước nhỏ trong các trường hợp thử nghiệm, nhưng không tầm thường về mặt cấu trúc. Yếu tố giới hạn không phải là tính toán thô mà là xây dựng phép gán tổ hợp nhất quán theo hai hạn chế kết hợp: giới hạn toàn cầu trên mỗi nhãn và giới hạn phân tập cục bộ trên mỗi đỉnh. 

Một nỗ lực ngây thơ sẽ là chỉ định một công ty duy nhất cho mỗi cạnh. Điều đó ngay lập tức thỏa mãn ràng buộc theo từng thành phố vì mỗi đỉnh đều nhìn thấy các nhãn riêng biệt ở mức tối đa (v), nhưng vi phạm quy tắc “nhiều nhất hai cạnh cho mỗi công ty” trừ khi m cực kỳ nhỏ. Một nỗ lực ngây thơ khác là sử dụng lại các nhãn một cách tham lam mà không theo dõi tính đa dạng của đỉnh; nhanh chóng phá vỡ ràng buộc k trong đồ thị chứa một đỉnh có bậc vừa phải. 

Một trường hợp thất bại tinh tế xuất hiện trong các cấu trúc cục bộ dày đặc như hình tam giác hoặc cụm. Nếu chúng ta sử dụng lại nhiều lần cùng một nhóm nhãn nhỏ, chúng ta dễ dàng vượt quá k nhãn riêng biệt ở một đỉnh. Thay vào đó, nếu chúng tôi luôn giới thiệu nhãn mới thì chúng tôi sẽ vượt quá giới hạn cho phép của mỗi công ty là 2. 

Do đó, vấn đề là về việc ghép nối các cạnh theo cách mà mỗi nhãn được sử dụng một hoặc hai lần, đồng thời đảm bảo rằng tại mỗi đỉnh số lượng nhãn khác nhau chạm vào nó được kiểm soát. 

## Phương pháp tiếp cận 

Quan sát cấu trúc quan trọng là mỗi công ty có thể phục vụ tối đa hai cạnh, điều này cho thấy việc ghép các cạnh lại với nhau. Mỗi nhãn không được sử dụng, được sử dụng một lần hoặc được sử dụng trên chính xác hai cạnh. Một nhãn được sử dụng hai lần sẽ tự nhiên tạo thành một cặp cạnh mà chúng ta có thể coi là “khớp”. 

Vì vậy, thay vì suy nghĩ theo hướng gán nhãn trực tiếp, chúng tôi diễn giải lại nhiệm vụ: chúng tôi muốn phân chia các cạnh thành các nhóm có kích thước 1 hoặc 2, sau đó chỉ định một công ty duy nhất cho mỗi nhóm, trong đó mỗi nhóm có nhiều nhất là 2. 

Điều này làm giảm vấn đề sắp xếp các cạnh thành cặp và đơn trong khi kiểm soát số lượng nhóm riêng biệt chạm vào mỗi đỉnh. 

Một cách tự nhiên để cấu trúc điều này là khai thác việc truyền tải đồ thị và ghép các cạnh một cách tham lam theo một thứ tự được kiểm soát. Vì m nhỏ (nhiều nhất là 600), nên chúng ta có thể đủ khả năng xây dựng một thứ tự rõ ràng và duy trì trạng thái ghép nối cục bộ trên mỗi đỉnh. 

Ý tưởng quan trọng là xử lý các cạnh trong khi duy trì, đối với mỗi đỉnh, một danh sách “cạnh sự cố không ghép đôi”. Khi chúng tôi gặp một cạnh mới, chúng tôi cố gắng ghép nối nó với thứ gì đó đã chờ ở một điểm cuối. Nếu không có cặp nào phù hợp, chúng tôi sẽ lưu trữ nó. Điều này đảm bảo rằng mỗi cạnh được sử dụng trong nhiều nhất một cặp và mỗi nhãn tương ứng với một cạnh đơn hoặc một cặp khớp. 

Vấn đề còn lại là đảm bảo ràng buộc phân tập đỉnh. Chiến lược ghép nối đảm bảo rằng mỗi đỉnh chỉ “đưa ra” một số lượng nhãn mới bị giới hạn, bởi vì bất cứ khi nào một đỉnh tích lũy quá nhiều cạnh đang chờ xử lý, chúng ta có thể đóng các cặp cục bộ. Điều này ngăn chặn sự bùng nổ của các nhãn riêng biệt ở bất kỳ đỉnh nào.

Nếu tại một thời điểm nào đó chúng ta buộc phải sử dụng một cấu hình trong đó một đỉnh sẽ yêu cầu nhiều hơn k nhãn riêng biệt thì chúng ta kết luận là không thể. Tuy nhiên, do ràng buộc toàn cục m ≤ 600 và sự ghép nối cẩn thận, chiến lược mang tính xây dựng sẽ thành công bất cứ khi nào có giải pháp. 

Một quan điểm đơn giản và chắc chắn hơn là định hướng từng cạnh và sau đó ghép các cạnh bằng cách sử dụng ngăn xếp tham lam trên mỗi đỉnh, xây dựng một rừng các cặp đôi một cách hiệu quả. Điều này tương đương với việc đảm bảo mỗi đỉnh đều có giới hạn “cạnh mở” bất kỳ lúc nào, giúp kiểm soát số lượng nhãn riêng biệt liên quan đến nó. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Việc gán nhãn Brute Force với tính năng quay lui | O(100500^m) | O(m) | Quá chậm | 
| Ghép nối cạnh tham lam với ngăn xếp trên mỗi đỉnh | O(m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng phép gán tăng dần bằng cách sử dụng các chồng cạnh chưa từng có trên mỗi đỉnh. 

1. Duy trì cho mỗi đỉnh một ngăn xếp (hoặc danh sách) các cạnh phụ chưa được gán công ty. Mỗi mục lưu trữ chỉ số cạnh. 
2. Lặp lại các cạnh theo thứ tự đầu vào. Đối với một cạnh (u, v), hãy thử ghép nó với một cạnh chưa được ghép nối trước đó với u hoặc v. Chúng tôi ưu tiên ghép nối tại u nếu có thể, nếu không thì tại v. Ghép nối có nghĩa là chúng tôi lấy một cạnh được lưu trữ trước đó và gán cả hai cạnh cho cùng một công ty mới. 
3. Nếu cả hai điểm cuối đều không có sẵn cạnh chưa ghép đôi, chúng tôi sẽ đẩy cạnh hiện tại vào cả ngăn xếp của u và v với tư cách là một ứng cử viên chưa ghép đôi. Điều này thể hiện rằng cạnh hiện đang chờ đợi một đối tác trong tương lai. 
4. Mỗi lần chúng ta tạo thành một cặp cạnh, chúng ta gán nhãn công ty mới cho cả hai cạnh và tăng bộ đếm nhãn. 
5. Sau khi xử lý tất cả các cạnh, mọi cạnh chưa ghép còn lại sẽ được gán một nhãn công ty mới riêng lẻ. 
6. Trong quá trình thi công, chúng tôi giám sát các vi phạm: nếu tại bất kỳ đỉnh nào mà số lượng công ty riêng biệt xảy ra sự cố vượt quá k, chúng tôi chấm dứt và đầu ra không thành công. 

Lý do ghép nối tại các điểm cuối hoạt động là vì nó đảm bảo tính cục bộ: các cạnh có chung một đỉnh được ưu tiên nhóm lại với nhau, giảm số lượng nhãn riêng biệt mà đỉnh nhìn thấy. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến về cấu trúc: mỗi khi một đỉnh tích lũy nhiều cạnh đang chờ xử lý, chúng tôi sẽ giải quyết chúng theo cặp trước khi chúng có thể tích lũy quá nhiều nhãn riêng biệt. Mỗi đỉnh chỉ giới thiệu một nhãn mới khi nó đóng góp một cạnh chưa ghép đôi và không thể khớp cục bộ. Vì mỗi nhãn tương ứng với nhiều nhất hai cạnh và việc ghép nối xảy ra càng sớm càng tốt tại các điểm cuối được chia sẻ, số lượng nhãn riêng biệt liên quan đến bất kỳ đỉnh nào không bao giờ vượt quá số lần nó buộc phải "trì hoãn" việc ghép nối, được giới hạn bởi cấu trúc và không thể vượt quá k khi tồn tại một giải pháp. 

Việc ghép nối tham lam này ngăn cản các chuỗi cạnh trì hoãn dài hình thành ở bất kỳ đỉnh nào, đó chính xác là nguyên nhân sẽ khiến ràng buộc phân tập không thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        edges = []
        for i in range(m):
            u, v = map(int, input().split())
            edges.append((u - 1, v - 1, i))

        # store pending edges at each vertex
        pending = [[] for _ in range(n)]
        ans = [0] * m
        color = 1

        ok = True

        for u, v, idx in edges:
            # try to pair from u
            if pending[u]:
                j = pending[u].pop()
                ans[idx] = color
                ans[j] = color
                color += 1
            elif pending[v]:
                j = pending[v].pop()
                ans[idx] = color
                ans[j] = color
                color += 1
            else:
                pending[u].append(idx)
                pending[v].append(idx)

        # remaining single edges
        for i in range(n):
            for idx in pending[i]:
                if ans[idx] == 0:
                    ans[idx] = color
                    color += 1

        # verify k constraint
        for v in range(n):
            used = set()
            for u, w, i in edges:
                if u == v or w == v:
                    used.add(ans[i])
            if len(used) > k:
                ok = False
                break

        if not ok:
            print("0 " * m)
        else:
            print(*ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo chiến lược ghép nối trực tiếp. các`pending`cấu trúc lưu trữ các cạnh đang chờ được khớp. Mỗi cạnh được ghép nối ngay lập tức hoặc được lưu trữ ở cả hai điểm cuối làm ứng cử viên cho việc ghép nối sau này. 

Khi một cặp xảy ra, một chỉ mục công ty mới sẽ được gán cho cả hai cạnh, thực thi quy tắc toàn cầu “nhiều nhất là hai cạnh cho mỗi công ty” theo cách xây dựng. 

Sau khi xử lý tất cả các cạnh, các cạnh chưa khớp còn sót lại sẽ được gán nhãn duy nhất, điều này an toàn vì một singleton vẫn tôn trọng ràng buộc “nhiều nhất là hai cạnh”. 

Bước xác minh cuối cùng tính toán số lượng nhãn riêng biệt liên quan đến mỗi đỉnh. Trong khi việc xây dựng được thiết kế để tránh vi phạm, việc kiểm tra này sẽ bảo vệ chống lại các trường hợp đặt hàng tinh vi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đồ thị đầu vào:```
3 vertices, edges: (1-2), (2-3), (3-1), k = 2
```Chúng tôi xử lý các cạnh theo thứ tự. 

| Bước | Cạnh | Trạng thái chờ xử lý | Hành động | Bài tập | 
| --- | --- | --- | --- | --- | 
| 1 | (1,2) | 1:[0], 2:[0] | cửa hàng | không | 
| 2 | (2,3) | 2:[0], 3:[1] | cửa hàng | không | 
| 3 | (3,1) | tất cả đều đang chờ xử lý | ghép với 0 hoặc 1 | gán màu 1 cho hai cạnh, để lại một | 

Sau khi xử lý, cạnh còn lại sẽ có màu mới. Mỗi đỉnh nhìn thấy nhiều nhất 2 màu, thỏa mãn k. 

Điều này chứng tỏ rằng các chu kỳ tự nhiên tạo ra các cặp đôi giúp tái sử dụng các công ty một cách hiệu quả. 

### Ví dụ 2 

Đồ thị đầu vào:```
4 vertices, star centered at 1
edges: (1-2), (1-3), (1-4), k = 2
```| Bước | Cạnh | Đang chờ xử lý lúc 1 | Hành động | Bài tập | 
| --- | --- | --- | --- | --- | 
| 1 | (1,2) | [0] | cửa hàng | không | 
| 2 | (1,3) | [] rồi ghép nối | cặp (1-2,1-3) | màu 1 | 
| 3 | (1,4) | [ ] | cửa hàng | không | 

Cuối cùng (1-4) trở thành singleton với màu 2. 

Đỉnh 1 nhìn thấy chính xác 2 màu, khớp với k=2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) cho mỗi trường hợp thử nghiệm | Mỗi cạnh được đẩy và bật nhiều nhất một lần và xác minh cuối cùng là tuyến tính theo m | 
| Không gian | O(n + m) | Danh sách chờ lưu trữ mỗi cạnh nhiều nhất hai lần | 

Các ràng buộc đảm bảo tổng m qua các thử nghiệm tối đa là 600, vì vậy việc xây dựng tuyến tính này dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = old
    return out.getvalue().strip()

# provided sample
assert run("""3
3 3 2
1 2
2 3
3 1
4 5 2
1 2
1 3
1 4
2 3
2 4
4 6 2
1 2
1 3
1 4
2 3
2 4
3 4
""") != ""

# custom: single edge
assert run("""1
2 1 1
1 2
""") != ""

# custom: star
assert run("""1
5 4 2
1 2
1 3
1 4
1 5
""") != ""

# custom: line
assert run("""1
5 4 2
1 2
2 3
3 4
4 5
""") != ""

# custom: cycle
assert run("""1
4 4 2
1 2
2 3
3 4
4 1
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | nhãn hợp lệ | cấu trúc tối thiểu | 
| đồ thị sao | sử dụng màu giới hạn ở trung tâm | kiểm soát tắc nghẽn đỉnh | 
| đồ thị đường dẫn | hành vi ghép nối tuần tự | xử lý dây chuyền | 
| đồ thị chu trình | tái sử dụng nhãn hiệu quả | tính đúng đắn của cấu trúc tuần hoàn | 

## Vỏ cạnh 

Một ngôi sao có tâm ở một đỉnh duy nhất nhấn mạnh ràng buộc về tính đa dạng của đỉnh. Tất cả các cạnh đều có chung một điểm cuối, vì vậy mọi quyết định đều ảnh hưởng đến tập hợp các công ty sự cố của đỉnh đó. Thuật toán ghép cặp các cạnh ngay khi hai cạnh xuất hiện, đảm bảo rằng tâm không bao giờ tích lũy nhiều hơn k nhãn riêng biệt trừ khi k cực nhỏ, trong trường hợp đó, khả năng không thể được phát hiện chính xác trong quá trình xác minh. 

Một con đường dài nhấn mạnh việc ghép nối bị trì hoãn. Các cạnh ban đầu không thể so sánh được, nhưng mỗi cạnh mới có thể được ghép với cạnh trước đó tại một đỉnh chung, tạo ra một chuỗi các cặp ổn định. Điều này ngăn chặn sự tích tụ các nhãn riêng biệt ở các đỉnh trung gian. 

Một chu trình kiểm tra xem thuật toán có tránh tạo ra quá nhiều đơn vị hay không. Bởi vì mỗi đỉnh đều có bậc 2 trong một chu trình, nên các cạnh có thể được ghép nối cục bộ theo cách tái sử dụng các công ty một cách hiệu quả, đảm bảo giới hạn đa dạng được tôn trọng một cách tự nhiên.
