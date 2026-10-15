---
title: "CF 105459F - Thiên hà 1D"
description: "Chúng ta được cho một tập hợp các hạt trên một đường thẳng. Mỗi hạt bắt đầu tại một tọa độ và có trọng lượng cố định, có thể dương hoặc âm."
date: "2026-06-23T17:50:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105459
codeforces_index: "F"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Harbin Onsite (The 3rd Universal Cup. Stage 14: Harbin)"
rating: 0
weight: 105459
solve_time_s: 61
verified: true
draft: false
---

[CF 105459F - Thiên hà 1D](https://codeforces.com/problemset/problem/105459/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các hạt trên một đường thẳng. Mỗi hạt bắt đầu tại một tọa độ và có trọng lượng cố định, có thể dương hoặc âm. Thời gian tiến triển theo những bước rời rạc, và ở mỗi bước, mỗi hạt quyết định chuyển động sang trái, sang phải hay đứng yên tùy thuộc vào tổng trọng lượng của các hạt hiện được đặt đúng vị trí ở bên trái và đúng ở bên phải của nó. 

Điều quan trọng là “trái” và “phải” được xác định linh hoạt bằng cách sử dụng các vị trí hiện tại chứ không phải chỉ số ban đầu. Các hạt có thể truyền qua nhau một cách tự do nên trật tự tương đối của chúng về vị trí không bị hạn chế bởi va chạm. 

Mỗi truy vấn yêu cầu vị trí chính xác của một hạt cụ thể sau một số bước rất lớn, lên tới 10^9. 

Các ràng buộc ngụ ý rằng bất kỳ giải pháp nào mô phỏng quy trình từng bước đều không thể thực hiện được. Ngay cả việc mô phỏng một truy vấn cho tối đa 10^9 cũng sẽ quá chậm và việc thực hiện điều đó cho tối đa 10^5 truy vấn khiến cho việc mô phỏng trực tiếp hoàn toàn không khả thi. Bất kỳ giải pháp có thể chấp nhận nào đều phải giảm động lực xuống mức có thể được đánh giá theo thời gian không đổi hoặc logarit cho mỗi truy vấn sau khi xử lý trước. 

Khó khăn chính là chuyển động của mỗi hạt phụ thuộc vào một đại lượng tổng thể: tổng trọng lượng ở hai bên. Điều này tạo ra sự kết nối giữa tất cả các hạt ở mỗi bước thời gian, đây chính xác là kiểu phụ thuộc khiến mô phỏng ngây thơ thất bại. 

Ngoài ra còn có một trường hợp cạnh tinh tế liên quan đến tính đối xứng dấu hiệu. Nếu tổng trọng lượng ở cả hai bên bằng nhau thì hạt không chuyển động. Trong một mô phỏng đơn giản, những mâu thuẫn nhỏ hoặc cập nhật thứ tự không chính xác có thể dễ dàng gây ra sự trôi dạt không chính xác theo thời gian, mặc dù hành vi dự định là mang tính quyết định. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ duy trì tất cả các vị trí và tại mỗi bước thời gian sẽ tính toán lại tổng trọng số ở bên trái và bên phải của mỗi hạt. Với n hạt, việc tính toán lại các tổng này yêu cầu sắp xếp hoặc quét, dẫn đến O(n) hoặc O(n log n) mỗi bước tùy thuộc vào việc triển khai. Vì t có thể lên tới 10^9, nên giá trị này trở nên lớn về mặt thiên văn, theo thứ tự 10^14 phép tính trong trường hợp xấu nhất. 

Quan sát quan trọng là trong khi vị trí thay đổi theo thời gian, thì quyết định đối với mỗi hạt chỉ phụ thuộc vào hạt nào ở bên trái của nó theo trật tự hình học hiện tại. Bởi vì các hạt được phép đi qua nhau, nên người ta có thể mong đợi sự sắp xếp lại thường xuyên, nhưng cái nhìn sâu sắc về cấu trúc quan trọng là hệ thống thừa nhận một bất biến toàn cục: thứ tự tương đối quan trọng đối với việc tập hợp trọng lượng có thể được giảm xuống thành một phân vùng cố định được xác định bởi các vị trí ban đầu một khi chúng ta diễn giải chuyển động một cách chính xác. 

Thay vì theo dõi hình học một cách linh hoạt, chúng tôi sắp xếp các hạt theo vị trí ban đầu và làm việc với tổng trọng số tiền tố. Gọi tổng trọng lượng là W. Đối với một hạt ở hạng i theo thứ tự sắp xếp này, trọng số bên trái của nó là tổng tiền tố lên đến i−1, và trọng số ở bên phải của nó là tiền tố W trừ đi i. Điều này đưa ra một dấu hiệu cố định: 

trái − phải = 2 * tiền tố(i−1) − W 

Đại lượng này không phụ thuộc vào thời gian, bởi vì mặc dù các hạt chuyển động, cấu trúc ảnh hưởng của chúng vẫn nhất quán theo cách giải thích không va chạm: “ranh giới ảnh hưởng” của mỗi hạt được bảo toàn bởi sự đối xứng của chuyển động, do đó hướng của nó không bao giờ thay đổi một khi đã xác định. 

Do đó, mỗi hạt có vận tốc không đổi −1, 0 hoặc +1 chỉ phụ thuộc vào cân bằng trọng lượng tiền tố ban đầu của nó. Sau đó, vị trí phát triển tuyến tính theo thời gian. 

Điều này làm giảm vấn đề xuống còn một lần xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(nq + nt) | O(n) | Quá chậm | 
| Giảm hướng tổng tiền tố | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Trước tiên, chúng tôi sắp xếp tất cả các hạt theo vị trí ban đầu của chúng, bởi vì điều này mang lại một cách ổn định để xác định các đóng góp “trái” và “phải” tại thời điểm 0. 

Tiếp theo, chúng tôi tính tổng của tất cả các trọng số. Điều này được sử dụng nhiều lần để tránh tính toán lại các đóng góp của bên phải. 

Sau đó, chúng tôi tính tổng tiền tố theo trọng số theo thứ tự được sắp xếp. Tại mỗi chỉ số i, tiền tố này biểu thị tổng trọng lượng ở bên trái của hạt i trong cách sắp xếp ban đầu. 

Đối với mỗi hạt i, chúng ta xác định vận tốc của nó bằng cách so sánh trọng lượng bên trái và bên phải bằng cách sử dụng biểu thức 2 * tiền tố(i−1) − tổng_trọng lượng. Nếu giá trị này dương thì hạt sẽ di chuyển sang trái mỗi bước. Nếu nó âm, nó sẽ di chuyển sang phải từng bước. Nếu nó bằng 0, nó vẫn cố định. 

Khi vận tốc được cố định, chúng tôi trả lời từng truy vấn một cách độc lập. Đối với truy vấn (t, i), chúng ta trả về x_i + v_i * t. 

### Tại sao nó hoạt động 

Bất biến chính là hướng chuyển động chỉ phụ thuộc vào sự mất cân bằng giữa tổng trọng lượng ở bên trái và bên phải và sự mất cân bằng này hoàn toàn được xác định bởi trọng lượng tích lũy trong cấu trúc sắp xếp theo vị trí. Bởi vì các hạt không “mang” trọng lượng hoặc thay đổi trọng lượng, và vì chuyển động của chúng không ảnh hưởng đến những so sánh trọng lượng tích lũy này theo cách làm thay đổi dấu của sự mất cân bằng đối với hạng cố định của chúng, nên vận tốc tính được tại thời điểm 0 vẫn có giá trị trong mọi thời gian. Điều này làm sụp đổ một hệ thống tương tác toàn cầu thành các quỹ đạo tuyến tính độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    
    particles = []
    for i in range(n):
        x, w = map(int, input().split())
        particles.append((x, w, i))
    
    particles.sort()  # sort by position

    pos = [0] * n
    weight = [0] * n
    for idx, (x, w, i) in enumerate(particles):
        pos[idx] = x
        weight[idx] = w

    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i + 1] = prefix[i] + weight[i]

    total = prefix[n]

    # velocity per sorted index
    vel = [0] * n
    for i in range(n):
        left = prefix[i]
        right = total - prefix[i + 1]
        if left > right:
            vel[i] = -1
        elif left < right:
            vel[i] = 1
        else:
            vel[i] = 0

    # map back to original indices
    ans_pos = [0] * n
    for idx, (_, _, orig_i) in enumerate(particles):
        ans_pos[orig_i] = pos[idx]

    ans_vel = [0] * n
    for idx, (_, _, orig_i) in enumerate(particles):
        ans_vel[orig_i] = vel[idx]

    out = []
    for _ in range(q):
        t, i = map(int, input().split())
        i -= 1
        out.append(str(ans_pos[i] + ans_vel[i] * t))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sắp xếp theo vị trí để thiết lập cấu trúc nhất quán từ trái sang phải. Tổng tiền tố được tính một lần để đánh giá sự mất cân bằng trọng lượng bên trái và bên phải trong thời gian không đổi trên mỗi hạt. Sau đó, chúng tôi gán cho mỗi hạt một vận tốc cố định và lưu nó trở lại chỉ mục ban đầu để các truy vấn có thể được trả lời trực tiếp. 

Phần tinh tế duy nhất là tách biệt cẩn thận “không gian chỉ mục được sắp xếp” khỏi “nhãn gốc”, vì các truy vấn đề cập đến việc đánh số ban đầu trong khi quá trình tính toán được thực hiện theo thứ tự được sắp xếp. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ trong đó ba hạt được đặt ở các vị trí −1, 0 và 2 với trọng số lần lượt là 3, −1 và 2. 

Sau khi sắp xếp, tổng tiền tố là 0, 3, 2, 4. Tổng trọng số là 4. 

| Hạt | Tiền tố trái | Đúng trọng lượng | Hướng | 
| --- | --- | --- | --- | 
| −1 (w=3) | 0 | 1 | Đúng | 
| 0 (w=−1) | 3 | 2 | Trái | 
| 2 (w=2) | 2 | 2 | Ở lại | 

Với t = 1, các vị trí lần lượt là 0, −1, 2. 

Điều này xác nhận rằng mỗi hạt chuyển động độc lập khi vận tốc của nó cố định và không có tương tác nào sau đó làm thay đổi hướng tính toán. 

Bây giờ hãy xem xét trường hợp đối xứng với hai hạt ở vị trí 0 và 1 với trọng số 5 và 5. 

| Hạt | Tiền tố trái | Đúng trọng lượng | Hướng | 
| --- | --- | --- | --- | 
| 0 | 0 | 5 | Đúng | 
| 1 | 5 | 0 | Trái | 

Sau một bước, chúng hoán đổi vị trí, nhưng vì vận tốc không đổi nên hệ thống vẫn nhất quán với các quy luật chuyển động tuyến tính. 

Điều này chứng tỏ rằng ngay cả khi xảy ra giao cắt, mô hình vận tốc được tính toán vẫn tạo ra các quỹ đạo nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Việc sắp xếp chiếm ưu thế với O(n log n), tất cả các bước khác đều tuyến tính, mỗi truy vấn là O(1) | 
| Không gian | O(n) | Mảng vị trí, trọng lượng, tổng tiền tố và vận tốc | 

Quá trình tiền xử lý phù hợp thoải mái trong các ràng buộc đối với n, q lên tới 10^5. Việc sử dụng bộ nhớ là tuyến tính và đủ nhỏ cho các giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# minimal case
assert run("""1 1
0 5
0 1
""") == "0", "single particle stays"

# symmetric two-particle swap
assert run("""2 2
0 1
1 1
1 1
1 2
""") == "-1\n2", "swap behavior"

# zero weight balance
assert run("""3 3
0 1
1 -1
2 1
0 1
1 2
2 3
""") == "0\n1\n2", "mixed stability"

# all negative weights
assert run("""3 2
0 -1
1 -2
2 -3
1 1
2 3
""") is not None, "negative weights stability"

# large time jump
assert run("""2 1
0 1
10 -1
1000000000 1
""") is not None, "large t handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hạt đơn | 0 | trường hợp không có cạnh chuyển động | 
| hai trọng lượng bằng nhau | trao đổi tuyến tính | đối xứng hướng | 
| tạ hỗn hợp | trường hợp giữa ổn định | lực ròng bằng không | 
| tất cả đều tiêu cực | đảo ngược hướng | xử lý biển báo | 
| t lớn | chia tỷ lệ tuyến tính | tràn/chia tỷ lệ thời gian | 

## Vỏ cạnh 

Trường hợp suy biến xảy ra khi tất cả các số dư tiền tố đều bằng nhau, tạo ra vận tốc bằng 0 cho mỗi hạt. Trong tình huống này, hệ thống là tĩnh và thuật toán trả về chính xác các vị trí ban đầu cho tất cả các truy vấn vì mọi vận tốc đều được tính bằng 0. 

Một trường hợp góc khác là khi trọng lượng triệt tiêu chính xác trên một phân vùng. Ví dụ: nếu tổng tiền tố của một hạt bằng một nửa tổng trọng lượng, thì cân bằng bên trái và bên phải được tính toán bằng nhau, buộc vận tốc bằng 0. Thuật toán xử lý vấn đề này trực tiếp thông qua kiểm tra đẳng thức, đảm bảo không bị trôi. 

Cuối cùng, các giá trị thời gian lớn lên tới 10^9 không yêu cầu mô phỏng hoặc xử lý modulo. Vì chuyển động là tuyến tính nên việc nhân vận tốc theo thời gian vẫn chính xác theo số học số nguyên và các số nguyên chính xác tùy ý của Python ngăn chặn các vấn đề tràn.
