---
problem: 1388C
contest_id: 1388
problem_index: C
name: "Uncle Bogdan and Country Happiness"
contest_name: "Codeforces Round 660 (Div. 2)"
rating: 1800
tags: ["dfs and similar", "greedy", "math", "trees"]
answer: passed_samples
verified: true
solve_time_s: 155
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e786e-940c-83ec-aff8-b894ca81a057
---

#CF 1388C - Bác Bogdan và Hạnh Phúc Đất Nước 

**Đánh giá:** 1800 
**Tags:** dfs và những thứ tương tự, tham lam, toán học, cây cối 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e786e-940c-83ec-aff8-b894ca81a057 

--- 

##Giải pháp 

## Hiểu vấn đề 

Đất nước này là một cái cây gồm những thành phố bắt nguồn từ thành phố 1, và mọi người dân đều bắt đầu ngày mới ở thủ đô trước khi trở về thành phố quê hương của mình dọc theo con đường ngắn nhất duy nhất trên cây. Mỗi thành phố có một lượng dân số đã biết được phân chia giữa các thành phố và mỗi người đóng góp +1 hoặc −1 tùy thuộc vào việc họ đang có tâm trạng tốt hay xấu. 

Quan sát quan trọng là thời điểm duy nhất một người có thể ảnh hưởng đến chỉ số hạnh phúc của một thành phố là khi họ đi qua thành phố đó trên đường từ thủ đô về nhà. Vì tất cả các đường dẫn đều là các đường dẫn từ gốc tới nút đơn giản nên mỗi công dân đóng góp theo đúng một đường dẫn trong cây. 

Đối với mỗi thành phố, chúng ta có một giá trị mục tiêu h[i], giá trị này được coi là bằng tổng đóng góp cuối cùng của tất cả những người đi qua thành phố đó. Nhiệm vụ là xác định xem có tồn tại sự phân công tâm trạng (tốt hay xấu) và những thay đổi tâm trạng có thể xảy ra dọc theo các đường dẫn sao cho tất cả các giá trị h[i] này có thể đạt được đồng thời hay không. 

Các ràng buộc đẩy chúng ta tới một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Với tổng số lên tới 2⋅10^5 nút và tối đa 10^4 trường hợp thử nghiệm, bất kỳ giải pháp nào thực hiện nhiều hơn O(n) cho mỗi trường hợp thử nghiệm hoặc O(n log n) tổng thể đều gặp rủi ro. Một mô phỏng đơn giản theo dõi từng người riêng lẻ là không thể vì m có thể lớn tới 10^9. 

Trường hợp cạnh tinh tế xuất hiện khi một thành phố không có dân số nhưng có h[i] khác 0. Ví dụ: nếu một thành phố lá có p[i] = 0 nhưng h[i] = 5, thì điều này ngay lập tức là không thể vì không có con đường nào có thể tạo thêm người ở đó. Một tình huống phức tạp khác là khi các giá trị ở địa phương có vẻ nhất quán nhưng trên toàn cầu lại vi phạm việc bảo tồn dòng chảy từ cha mẹ đến con cái. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu sẽ đối xử với mỗi người một cách riêng biệt. Chúng ta có thể tưởng tượng việc gán cho mỗi người trong số m người một tâm trạng và mô phỏng đường đi của họ từ gốc đến nhà của họ, đánh dấu những đóng góp tại mỗi nút được truy cập. Điều này đúng về mặt khái niệm nhưng không khả thi ngay lập tức vì mỗi người có thể đi qua tới các nút O(n), đưa ra các hoạt động O(nm), vượt xa giới hạn khi m đạt 10^9. 

Chúng ta cần chuyển từ suy nghĩ về các cá nhân sang suy nghĩ về các dòng chảy tổng hợp. Thay vì theo dõi mọi người, chúng tôi theo dõi xem có bao nhiêu người phải đi qua từng cây con và bao nhiêu người trong số họ được phép trở thành xấu. Điểm mấu chốt là cấu trúc cây buộc phải phân rã đệ quy: mỗi cây con phải “giải thích” mức độ hạnh phúc cần thiết của nó chỉ bằng cách sử dụng những người bên trong nó cộng với những gì diễn ra từ phía trên. 

Chúng tôi root cây ở mức 1 và thực hiện DFS. Tại mỗi nút, chúng tôi tính toán có bao nhiêu người trong cây con của nó (gọi là tổng số) và bao nhiêu người trong số họ phải tốt để đáp ứng các ràng buộc về hạnh phúc cần thiết. Vấn đề trở thành việc kiểm tra tính khả thi xem liệu số lượng hàng hóa cần thiết này có thể nằm trong giới hạn hợp lệ ở mỗi cây con hay không. 

Tại nút u, nếu chúng ta đã biết rằng một cây con con yêu cầu nhiều “đóng góp tốt” hơn những người có sẵn trong cây con đó thì việc cấu hình là không thể. Tương tự, nếu số lượng người giỏi cần thiết là âm hoặc vượt quá tổng số người sẵn có thì điều đó là không thể. 

Điều này dẫn đến việc duyệt thứ tự sau tham lam trong đó mỗi cây con tổng hợp các yêu cầu của nó trở lên và tính khả thi được kiểm tra cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng cá nhân) | O(nm) | O(nm) | Quá chậm | 
| Cây DFS với tập hợp cây con | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở nút 1 và coi nó như một cấu trúc được định hướng từ cha mẹ đến con cái.

1. Xây dựng danh sách kề cho cây. Điều này cho phép truyền tải hiệu quả trong O(n). 
2. Chạy DFS từ thư mục gốc. Đối với mỗi nút u, chúng tôi tính toán hai giá trị từ các nút con của nó: tổng số người trong cây con của nó và số lượng “người tốt cần thiết” phải tồn tại trong cây con đó để đáp ứng tất cả các ràng buộc bên dưới. 

Lý do chúng tôi sử dụng phương pháp truyền tải theo thứ tự sau là vì tính khả thi của một nút phụ thuộc hoàn toàn vào cách các nút con của nó phân phối những đóng góp cần thiết của chúng. 
3. Tại mỗi nút u, khởi tạo Total_people = p[u] và Need_good = 0. 
4. Với mọi con v của u, hãy tính đệ quy (total_v, good_v) của nó. Thêm Total_v vào Total_people và thêm Good_v vào Need_good. 

Sự kết hợp này phản ánh rằng tất cả mọi người trong cây con con phải đi qua u trước khi đến gốc, do đó các ràng buộc của họ tích lũy lên trên. 
5. Sau khi xử lý phần tử con, hãy kết hợp ràng buộc hạnh phúc h[u]. Hãy coi số dư hiện tại là sự khác biệt giữa những đóng góp tốt và xấu đi qua bạn. Vì tổng số người trong cây con là cố định nên chúng tôi diễn giải require_good là số người phải duy trì tốt trong cây con này để đạt được h[u]. 

Chúng ta dịch h[u] thành một ràng buộc về số lượng người tốt phải tồn tại trong cây con u: 

bắt buộc_good = (tổng_người + h[u]) / 2. 

Điều này xuất phát từ việc giải quyết: 

tốt - xấu = h[u] 

tốt + xấu = tổng_người 

ngụ ý: 

tốt = (total_people + h[u]) / 2 
6. Nếu (total_people + h[u]) là số lẻ hoặc require_good nằm ngoài [0, Total_people] thì không thể trả về. 
7. Trả về (tổng_người, yêu cầu_tốt) trở lên cho cha mẹ. 
8. Nếu root khả thi, hãy trả lời CÓ. 

### Tại sao nó hoạt động 

Mỗi cây con được coi như một hệ thống khép kín trong đó tất cả mọi người bên trong nó đóng góp chính xác một lần cho mỗi nút trên đường dẫn tới gốc. Bởi vì các đóng góp có tính cộng và các đường đi là duy nhất trong một cây, mỗi cây con chỉ cần đảm bảo rằng số lượng các phép gán tốt mà nó yêu cầu phù hợp với tổng dân số của nó và chênh lệch ròng yêu cầu h[u]. DFS đảm bảo rằng tất cả các ràng buộc con cháu đều được thỏa mãn trước khi nút cam kết với một giá trị, do đó mọi vi phạm phải được phát hiện ở ranh giới cây con sớm nhất có thể. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(10**7)
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    p = list(map(int, input().split()))
    h = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        g[a].append(b)
        g[b].append(a)

    def dfs(u, parent):
        total = p[u]
        good = 0

        for v in g[u]:
            if v == parent:
                continue
            t_v, g_v = dfs(v, u)
            total += t_v
            good += g_v

        if (total + h[u]) % 2 != 0:
            return -1, -1

        req_good = (total + h[u]) // 2

        if req_good < 0 or req_good > total:
            return -1, -1

        if good > req_good:
            return -1, -1

        return total, req_good

    ok_total, ok_good = dfs(0, -1)
    print("YES" if ok_total != -1 else "NO")

t = int(input())
for _ in range(t):
    solve()
```DFS trả về hai giá trị cho mỗi nút: tổng dân số trong cây con của nó và số lượng người tốt cần thiết để đáp ứng ràng buộc hạnh phúc tại nút đó. Việc kiểm tra tính chẵn lẻ buộc phải tồn tại sự phân chia hợp lý giữa người tốt và người xấu. 

Một lỗi phổ biến là bỏ qua điều kiện chẵn lẻ. Vì tốt và xấu là số nguyên nên (tổng + h[u]) phải là số chẵn. Một điểm tinh tế khác là đảm bảo rằng require_good được tính toán không vượt quá tổng số cây con hoặc giảm xuống dưới 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3 3
1 1 1
1 1 -1
1 2
1 3
```Cây: 

1 là root, con 2 và 3. 

| Nút | tổng cây con | h[u] | bắt buộc_tốt | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 1 | được | 
| 3 | 1 | -1 | 0 | được | 
| 1 | 3 | 1 | 2 | được | 

Cả hai phần tử con đều hợp lệ và ràng buộc gốc là nhất quán. 

Điều này cho thấy các giá trị của cây con có giá trị độc lập và kết hợp rõ ràng ở gốc như thế nào. 

### Ví dụ 2 

đầu vào:```
1
2 2
1 1
-1 1
1 2
```| Nút | tổng cây con | h[u] | bắt buộc_tốt | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 1 | được | 
| 1 | 2 | -1 | 0,5 | không hợp lệ | 

Tại gốc, tính chẵn lẻ không thành công ngay lập tức vì tổng + h là số lẻ. Điều này ghi lại trường hợp tồn tại tính nhất quán của cây con cục bộ nhưng tính khả thi toàn cầu bị phá vỡ do các ràng buộc về số nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút và cạnh được truy cập một lần trong DFS | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy | 

Tổng số nút trong các trường hợp thử nghiệm tối đa là 2⋅10^5, do đó DFS tuyến tính cho mỗi trường hợp thử nghiệm vẫn nằm trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    output = []
    def input():
        return sys.stdin.readline().strip()

    def solve_all():
        t = int(input())
        for _ in range(t):
            n, m = map(int, input().split())
            p = list(map(int, input().split()))
            h = list(map(int, input().split()))
            g = [[] for _ in range(n)]
            for _ in range(n - 1):
                a, b = map(int, input().split())
                g[a-1].append(b-1)
                g[b-1].append(a-1)

            sys.setrecursionlimit(10**7)

            def dfs(u, par):
                total = p[u]
                good = 0
                for v in g[u]:
                    if v == par:
                        continue
                    t, gg = dfs(v, u)
                    total += t
                    good += gg
                if (total + h[u]) % 2 != 0:
                    return -1, -1
                req = (total + h[u]) // 2
                if req < 0 or req > total:
                    return -1, -1
                if good > req:
                    return -1, -1
                return total, req

            ok = dfs(0, -1)[0] != -1
            output.append("YES" if ok else "NO")

    solve_all()
    return "\n".join(output)

# provided samples
assert run("""2
7 4
1 0 1 1 0 1 0
4 0 0 -1 0 -1 0
1 2
1 3
1 4
3 5
3 6
3 7
5 11
1 2 5 2 1
-11 -2 -6 -2 -1
1 2
1 3
1 4
3 5
""") == "YES\nYES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | CÓ | tính đúng đắn của trường hợp cơ sở | 
| cây sao có tính chẵn lẻ không hợp lệ | KHÔNG | phát hiện ràng buộc chẵn lẻ | 
| giá trị nhất quán chuỗi sâu | CÓ | tính chính xác của việc truyền bá | 
| yêu cầu cây con không nhất quán | KHÔNG | thất bại khả thi cục bộ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một cây con có tổng thể bằng 0 nhưng yêu cầu hạnh phúc khác 0. Ví dụ, một nút lá có p[i] = 0 và h[i] = 2 ngay lập tức bị lỗi do không tồn tại sự phân công trạng thái tốt hay xấu. DFS nắm bắt được điều này khi tính toán (tổng + h[i])/2, tạo ra giá trị âm hoặc phân số. 

Một trường hợp quan trọng khác là khi tất cả các ràng buộc đều có hiệu lực cục bộ nhưng nút cha lại yêu cầu ít người giỏi hơn số người đã được con cố định. Trong tình huống đó, good > require_good sẽ gây ra lỗi, đảm bảo rằng các cam kết con không bị vi phạm bởi sự tổng hợp ở cấp độ cao hơn.
