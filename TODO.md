# TODO - Issue #397: New Endpoint GET /transaction/:hash/effects

- [x] Inspect existing transaction routes and response/normalization utilities
- [x] Implement GET /transaction/:hash/effects route
  - [x] Validate :hash is 64-char hex before Horizon call
  - [x] Fetch all effects for transaction hash via Horizon
  - [x] Normalize each effect with: effectId, type, account, createdAt, plus type-specific fields (best-effort)
  - [x] Return { success: true, data: { effects: [...], total } }
  - [x] Return 404 with clear message when transaction hash does not exist
- [x] Add/Update tests for the new endpoint (shape + validation + 404 behavior)

- [x] Ensure routing is registered in src/index.js (and docs list if applicable)
- [x] Run targeted unit tests for the endpoint only (no build)
TODO: Add Output Encoding to Error Message Interpolation

Description:
Safely encode all user-provided values interpolated into error messages to prevent malformed JSON responses and unexpected rendering when inputs contain special characters.

Requirements:

- Add a centralized output-encoding/escaping utility for user-controlled values.
- Apply encoding to all error messages that echo:
  - Account IDs
  - Asset codes
  - Policy names
- Ensure the following characters are safely escaped:
  - "<"
  - ">"
  - """
  - "\"
  - "/"
- Preserve the JSON structure of error responses regardless of malicious input.
- Ensure encoded values remain readable enough for debugging while being safe for client rendering.

Tests:

- Add unit/integration tests covering each special character for:
  - Account ID error messages
  - Asset code error messages
  - Policy name error messages
- Test combinations of special characters and malicious payloads.
- Verify every response remains valid JSON and the echoed value cannot alter the response structure.
- Verify existing normal error messages remain unchanged for safe inputs.

Acceptance Criteria:

- No user-controlled value is interpolated into an error response without encoding.
- "<", ">", """, "\", and "/" are properly escaped.
- Malicious input cannot break JSON encoding or client-side rendering.
- All relevant tests pass.
- Existing API error response formats remain backward compatible.
TODO: Add Output Encoding to Error Message Interpolation

1. Project Discovery

1. [ ] Inspect the repository structure.
2. [ ] Identify the backend/API application.
3. [ ] Identify the framework used by the backend.
4. [ ] Identify the application entry point.
5. [ ] Identify the API route directory.
6. [ ] Identify the controller directory.
7. [ ] Identify the service directory.
8. [ ] Identify the utility/helper directory.
9. [ ] Identify existing error-handling utilities.
10. [ ] Identify existing API response helpers.
11. [ ] Identify all custom application error classes.
12. [ ] Identify all error response types.
13. [ ] Identify all locations where error messages are interpolated.
14. [ ] Search for template literals in error messages.
15. [ ] Search for string concatenation in error messages.
16. [ ] Search for account ID interpolation.
17. [ ] Search for asset code interpolation.
18. [ ] Search for policy name interpolation.
19. [ ] Search for other user-controlled values in errors.
20. [ ] Search for "not found" error messages.
21. [ ] Search for "invalid" error messages.
22. [ ] Search for "unsupported" error messages.
23. [ ] Search for "already exists" error messages.
24. [ ] Search for validation errors containing request values.
25. [ ] Search for thrown errors containing request values.
26. [ ] Search for HTTP 400 responses containing request values.
27. [ ] Search for HTTP 404 responses containing request values.
28. [ ] Search for HTTP 409 responses containing request values.
29. [ ] Search for HTTP 422 responses containing request values.
30. [ ] Search for HTTP 500 responses that might echo input.
31. [ ] Review existing test files.
32. [ ] Identify tests for account-related errors.
33. [ ] Identify tests for asset-related errors.
34. [ ] Identify tests for policy-related errors.
35. [ ] Identify tests for API error responses.
36. [ ] Identify existing security-related tests.
37. [ ] Identify existing encoding utilities.
38. [ ] Check whether an HTML/entity encoder already exists.
39. [ ] Check whether an escaping dependency already exists.
40. [ ] Define the implementation scope.

2. Threat Model

41. [ ] Document that user input can appear in error messages.
42. [ ] Identify account IDs as user-controlled input.
43. [ ] Identify asset codes as user-controlled input.
44. [ ] Identify policy names as user-controlled input.
45. [ ] Identify request parameters that can reach errors.
46. [ ] Identify request body fields that can reach errors.
47. [ ] Identify query parameters that can reach errors.
48. [ ] Identify path parameters that can reach errors.
49. [ ] Identify values retrieved from external input that are echoed.
50. [ ] Determine where unsafe interpolation occurs.
51. [ ] Identify JSON-breaking characters.
52. [ ] Identify HTML-sensitive characters.
53. [ ] Identify quote characters.
54. [ ] Identify slash characters.
55. [ ] Identify angle brackets.
56. [ ] Identify backslash handling requirements.
57. [ ] Consider newline handling where applicable.
58. [ ] Consider carriage-return handling where applicable.
59. [ ] Consider tab characters where applicable.
60. [ ] Consider Unicode input.
61. [ ] Consider repeated special characters.
62. [ ] Consider very long malicious values.
63. [ ] Consider nested quote characters.
64. [ ] Consider combinations of special characters.
65. [ ] Consider encoded input supplied by clients.
66. [ ] Consider double-encoded input.
67. [ ] Consider malformed input.
68. [ ] Consider empty input.
69. [ ] Consider whitespace-only input.
70. [ ] Ensure the mitigation does not rely solely on client-side encoding.

3. Define Encoding Strategy

71. [ ] Determine the required output encoding format.
72. [ ] Confirm the encoding strategy is appropriate for JSON responses.
73. [ ] Confirm the strategy safely handles "<".
74. [ ] Confirm the strategy safely handles ">".
75. [ ] Confirm the strategy safely handles """.
76. [ ] Confirm the strategy safely handles "\".
77. [ ] Confirm the strategy safely handles "/".
78. [ ] Confirm the strategy does not produce invalid JSON.
79. [ ] Confirm encoded values remain deterministic.
80. [ ] Confirm encoded values can safely appear inside JSON strings.
81. [ ] Determine whether HTML entities are required.
82. [ ] Determine whether JSON string escaping is required.
83. [ ] Determine whether both JSON and HTML contexts need separate handling.
84. [ ] Avoid using HTML encoding blindly for JSON unless required by project behavior.
85. [ ] Avoid using URL encoding for JSON error messages.
86. [ ] Avoid using Base64 as an output-encoding substitute.
87. [ ] Avoid relying on manual backslash replacement where a trusted encoder exists.
88. [ ] Check whether the framework already safely serializes JSON values.
89. [ ] Determine whether the actual vulnerability is message construction or JSON serialization.
90. [ ] Ensure the implementation matches the acceptance criteria.
91. [ ] Define a single reusable encoding function where appropriate.
92. [ ] Give the encoding helper a descriptive name.
93. [ ] Ensure the helper accepts string input.
94. [ ] Define behavior for null values if applicable.
95. [ ] Define behavior for undefined values if applicable.
96. [ ] Define behavior for non-string values if applicable.
97. [ ] Ensure the helper does not mutate original input.
98. [ ] Ensure the helper does not modify database values.
99. [ ] Ensure the helper only affects output representation.
100. [ ] Document the chosen encoding strategy.

4. Encoding Utility

101. [ ] Create a reusable output-encoding utility if one does not exist.
102. [ ] Place the utility in the project's appropriate helper directory.
103. [ ] Follow existing project naming conventions.
104. [ ] Add strict typing.
105. [ ] Keep the utility small and focused.
106. [ ] Avoid adding unrelated functionality.
107. [ ] Implement safe handling for "<".
108. [ ] Implement safe handling for ">".
109. [ ] Implement safe handling for """.
110. [ ] Implement safe handling for "\".
111. [ ] Implement safe handling for "/".
112. [ ] Handle characters in the correct order if replacements are used.
113. [ ] Prevent replacement operations from double-encoding output.
114. [ ] Ensure already-safe characters remain unchanged.
115. [ ] Ensure ordinary alphanumeric input remains readable.
116. [ ] Ensure spaces remain valid.
117. [ ] Ensure hyphens remain valid.
118. [ ] Ensure underscores remain valid.
119. [ ] Ensure periods remain valid.
120. [ ] Ensure colons remain valid when legitimate.
121. [ ] Ensure Unicode characters remain supported.
122. [ ] Ensure non-ASCII account IDs remain safe.
123. [ ] Ensure non-ASCII asset codes remain safe.
124. [ ] Ensure non-ASCII policy names remain safe.
125. [ ] Ensure empty strings are handled.
126. [ ] Ensure repeated characters are handled.
127. [ ] Ensure long strings are handled.
128. [ ] Ensure the utility does not throw on normal input.
129. [ ] Ensure the utility does not execute user input.
130. [ ] Ensure the utility cannot introduce executable markup.
131. [ ] Ensure the utility cannot introduce malformed JSON.
132. [ ] Add unit tests for the encoding utility.
133. [ ] Test each required special character individually.
134. [ ] Test combinations of special characters.
135. [ ] Test ordinary strings.
136. [ ] Test empty strings.
137. [ ] Test Unicode strings.
138. [ ] Test repeated special characters.
139. [ ] Test long malicious strings.
140. [ ] Confirm all utility tests pass.

5. Account ID Error Messages

141. [ ] Locate account ID lookup errors.
142. [ ] Locate account-not-found errors.
143. [ ] Locate invalid-account errors.
144. [ ] Locate account validation errors.
145. [ ] Locate account-related authorization errors containing IDs.
146. [ ] Locate account-related conflict errors containing IDs.
147. [ ] Identify every account ID interpolation point.
148. [ ] Apply output encoding before interpolation.
149. [ ] Ensure raw account IDs are never inserted directly.
150. [ ] Preserve the existing error message wording.
151. [ ] Preserve the existing error type.
152. [ ] Preserve the existing HTTP status code.
153. [ ] Preserve the existing response structure.
154. [ ] Ensure only the echoed value is encoded.
155. [ ] Ensure surrounding message text remains unchanged.
156. [ ] Test account ID containing "<".
157. [ ] Test account ID containing ">".
158. [ ] Test account ID containing """.
159. [ ] Test account ID containing "\".
160. [ ] Test account ID containing "/".
161. [ ] Test account ID containing multiple special characters.
162. [ ] Test account ID containing quotes and slashes together.
163. [ ] Test account ID containing angle brackets.
164. [ ] Test account ID containing backslashes.
165. [ ] Test account ID containing malicious markup-like content.
166. [ ] Test account ID containing JSON-like content.
167. [ ] Test account ID containing escaped sequences.
168. [ ] Test account ID containing Unicode.
169. [ ] Test normal account IDs.
170. [ ] Verify account error JSON remains valid.

6. Asset Code Error Messages

171. [ ] Locate asset code lookup errors.
172. [ ] Locate asset-not-found errors.
173. [ ] Locate invalid-asset errors.
174. [ ] Locate unsupported asset errors.
175. [ ] Locate asset validation errors.
176. [ ] Identify every asset code interpolation point.
177. [ ] Apply output encoding before interpolation.
178. [ ] Ensure raw asset codes are never directly interpolated.
179. [ ] Preserve existing error message wording.
180. [ ] Preserve existing status codes.
181. [ ] Preserve existing error types.
182. [ ] Preserve existing JSON structure.
183. [ ] Test asset code containing "<".
184. [ ] Test asset code containing ">".
185. [ ] Test asset code containing """.
186. [ ] Test asset code containing "\".
187. [ ] Test asset code containing "/".
188. [ ] Test asset code containing all required special characters.
189. [ ] Test asset code containing repeated special characters.
190. [ ] Test asset code containing quote combinations.
191. [ ] Test asset code containing slash combinations.
192. [ ] Test asset code containing markup-like input.
193. [ ] Test asset code containing JSON-like input.
194. [ ] Test asset code containing Unicode.
195. [ ] Test normal asset codes.
196. [ ] Verify asset error responses remain valid JSON.
197. [ ] Verify asset error messages remain readable.
198. [ ] Verify encoding does not alter normal asset codes.
199. [ ] Verify no route behavior changes.
200. [ ] Verify all asset error tests pass.

7. Policy Name Error Messages

201. [ ] Locate policy lookup errors.
202. [ ] Locate policy-not-found errors.
203. [ ] Locate invalid-policy errors.
204. [ ] Locate policy validation errors.
205. [ ] Locate policy conflict errors.
206. [ ] Identify every policy name interpolation point.
207. [ ] Apply output encoding before interpolation.
208. [ ] Ensure raw policy names are never directly interpolated.
209. [ ] Preserve existing policy error wording.
210. [ ] Preserve existing HTTP status codes.
211. [ ] Preserve existing error types.
212. [ ] Preserve existing response structure.
213. [ ] Test policy name containing "<".
214. [ ] Test policy name containing ">".
215. [ ] Test policy name containing """.
216. [ ] Test policy name containing "\".
217. [ ] Test policy name containing "/".
218. [ ] Test policy name containing all required special characters.
219. [ ] Test policy names containing repeated special characters.
220. [ ] Test policy names containing nested quotes.
221. [ ] Test policy names containing markup-like strings.
222. [ ] Test policy names containing JSON-like strings.
223. [ ] Test policy names containing Unicode.
224. [ ] Test normal policy names.
225. [ ] Verify policy error JSON remains valid.
226. [ ] Verify policy messages remain readable.
227. [ ] Verify encoding does not alter ordinary policy names.
228. [ ] Verify no policy behavior changes.
229. [ ] Verify all policy error tests pass.
230. [ ] Confirm policy error coverage is complete.

8. Global Error Interpolation Audit

231. [ ] Search the entire project for interpolated error messages.
232. [ ] Search for template literals inside thrown errors.
233. [ ] Search for string concatenation inside errors.
234. [ ] Search for interpolated API response messages.
235. [ ] Search for "message:" properties containing variables.
236. [ ] Search for "error.message" construction.
237. [ ] Search for "throw new Error".
238. [ ] Search for framework-specific HTTP errors.
239. [ ] Search for validation error construction.
240. [ ] Search for controller-level errors.
241. [ ] Search for service-level errors.
242. [ ] Search for repository-level errors.
243. [ ] Search for request parameter interpolation.
244. [ ] Search for query parameter interpolation.
245. [ ] Search for body parameter interpolation.
246. [ ] Search for path parameter interpolation.
247. [ ] Search for headers being echoed.
248. [ ] Search for external identifiers being echoed.
249. [ ] Identify all user-controlled error values.
250. [ ] Categorize each interpolation as safe or unsafe.
251. [ ] Apply encoding to every unsafe interpolation.
252. [ ] Avoid encoding trusted static text unnecessarily.
253. [ ] Avoid encoding values that are never returned.
254. [ ] Avoid changing unrelated success responses.
255. [ ] Avoid changing database storage.
256. [ ] Avoid changing request validation semantics.
257. [ ] Avoid changing internal logs unless required.
258. [ ] Confirm internal exceptions are unaffected where appropriate.
259. [ ] Confirm public API error messages are protected.
260. [ ] Confirm the audit is complete.

9. JSON Integrity Tests

261. [ ] Create tests specifically for JSON integrity.
262. [ ] Test account ID containing a double quote.
263. [ ] Test account ID containing a backslash.
264. [ ] Test account ID containing slash characters.
265. [ ] Test account ID containing angle brackets.
266. [ ] Test asset code containing a double quote.
267. [ ] Test asset code containing a backslash.
268. [ ] Test asset code containing slash characters.
269. [ ] Test asset code containing angle brackets.
270. [ ] Test policy name containing a double quote.
271. [ ] Test policy name containing a backslash.
272. [ ] Test policy name containing slash characters.
273. [ ] Test policy name containing angle brackets.
274. [ ] Test combinations across all three input types.
275. [ ] Parse every generated response as JSON.
276. [ ] Assert JSON parsing succeeds.
277. [ ] Assert the response contains the expected "error" object.
278. [ ] Assert the expected error type remains present.
279. [ ] Assert the expected error message remains present.
280. [ ] Assert no raw malformed JSON is returned.
281. [ ] Assert no unescaped quote breaks the response.
282. [ ] Assert no backslash corrupts the response.
283. [ ] Assert special-character combinations remain valid.
284. [ ] Test malicious JSON fragments.
285. [ ] Test embedded JSON objects as input.
286. [ ] Test embedded JSON arrays as input.
287. [ ] Test strings containing escaped quotes.
288. [ ] Test strings containing repeated escape characters.
289. [ ] Test strings containing angle-bracket markup.
290. [ ] Confirm every response remains parseable.

10. Security Regression Tests

291. [ ] Add regression coverage for previously vulnerable interpolation points.
292. [ ] Verify account IDs cannot alter JSON structure.
293. [ ] Verify asset codes cannot alter JSON structure.
294. [ ] Verify policy names cannot alter JSON structure.
295. [ ] Verify encoded values cannot create executable markup.
296. [ ] Verify encoded values cannot terminate JSON strings.
297. [ ] Verify encoded values cannot add arbitrary JSON fields.
298. [ ] Verify encoded values cannot remove existing JSON fields.
299. [ ] Verify encoded values cannot modify the error type.
300. [ ] Verify encoded values cannot modify the success flag.
301. [ ] Verify encoded values cannot inject additional properties.
302. [ ] Verify encoded values cannot inject arbitrary nested JSON.
303. [ ] Verify encoded values cannot affect HTTP response structure.
304. [ ] Verify encoded values cannot affect status codes.
305. [ ] Verify encoded values cannot affect headers unexpectedly.
306. [ ] Verify encoded values remain data rather than executable content.
307. [ ] Verify normal input behavior remains unchanged.
308. [ ] Verify malformed input produces a controlled error.
309. [ ] Verify unusually long input does not crash the encoder.
310. [ ] Verify repeated malicious requests do not cause application errors.
311. [ ] Verify encoding occurs consistently.
312. [ ] Verify no vulnerable interpolation remains.
313. [ ] Run a second source-code audit after implementation.
314. [ ] Compare all identified interpolation points against the implementation.
315. [ ] Confirm every relevant point has coverage.

11. Unit Test Organization

316. [ ] Group encoding tests by input type.
317. [ ] Group account ID tests together.
318. [ ] Group asset code tests together.
319. [ ] Group policy name tests together.
320. [ ] Add clear test descriptions.
321. [ ] Make each special-character test independently identifiable.
322. [ ] Test "<" independently.
323. [ ] Test ">" independently.
324. [ ] Test """ independently.
325. [ ] Test "\" independently.
326. [ ] Test "/" independently.
327. [ ] Add combined-character tests.
328. [ ] Add normal-input tests.
329. [ ] Add empty-input tests where relevant.
330. [ ] Add Unicode tests where relevant.
331. [ ] Ensure test fixtures are deterministic.
332. [ ] Avoid dependence on external services.
333. [ ] Avoid dependence on production configuration.
334. [ ] Ensure test cleanup is performed.
335. [ ] Ensure environment state is restored.
336. [ ] Ensure tests do not modify persistent data unnecessarily.
337. [ ] Ensure tests run independently.
338. [ ] Ensure tests pass repeatedly.
339. [ ] Ensure tests cover both helper and API behavior.
340. [ ] Confirm test names describe expected security behavior.

12. API-Level Verification

341. [ ] Start the application locally.
342. [ ] Identify endpoints that trigger account errors.
343. [ ] Send malicious account ID input.
344. [ ] Confirm the API returns the expected error.
345. [ ] Confirm the response can be parsed as JSON.
346. [ ] Send malicious asset code input.
347. [ ] Confirm the API returns the expected error.
348. [ ] Confirm the response can be parsed as JSON.
349. [ ] Send malicious policy name input.
350. [ ] Confirm the API returns the expected error.
351. [ ] Confirm the response can be parsed as JSON.
352. [ ] Test each special character individually.
353. [ ] Test all characters together.
354. [ ] Test nested special-character combinations.
355. [ ] Test normal values.
356. [ ] Compare normal response behavior before and after the change.
357. [ ] Confirm status codes remain unchanged.
358. [ ] Confirm error types remain unchanged.
359. [ ] Confirm response structure remains unchanged.
360. [ ] Confirm only unsafe interpolated values are encoded.

13. Full Project Validation

361. [ ] Run the complete unit test suite.
362. [ ] Run integration tests.
363. [ ] Run API tests.
364. [ ] Run security-related tests.
365. [ ] Run linting.
366. [ ] Run formatting checks.
367. [ ] Run TypeScript type checking.
368. [ ] Run the production build.
369. [ ] Confirm no new compilation errors.
370. [ ] Confirm no new lint errors.
371. [ ] Confirm no new formatting errors.
372. [ ] Confirm no unrelated tests fail.
373. [ ] Investigate any pre-existing failures.
374. [ ] Confirm all new tests pass.
375. [ ] Confirm all acceptance criteria are covered.
376. [ ] Perform a final source-code search for unsafe interpolation.
377. [ ] Review all changed files.
378. [ ] Remove unused imports.
379. [ ] Remove unnecessary helper functions.
380. [ ] Remove debugging statements.
381. [ ] Remove temporary test code.
382. [ ] Ensure no sensitive data was added.
383. [ ] Ensure no dependency was added unnecessarily.
384. [ ] Ensure the implementation follows project conventions.
385. [ ] Ensure the implementation is backwards compatible.

14. Final Acceptance Checklist

386. [ ] User-controlled account IDs are safely encoded before error-message interpolation.
387. [ ] User-controlled asset codes are safely encoded before error-message interpolation.
388. [ ] User-controlled policy names are safely encoded before error-message interpolation.
389. [ ] "<" is safely handled.
390. [ ] ">" is safely handled.
391. [ ] """ is safely handled.
392. [ ] "\" is safely handled.
393. [ ] "/" is safely handled.
394. [ ] Malicious input cannot break JSON structure.
395. [ ] Error response JSON remains parseable.
396. [ ] Error types remain unchanged.
397. [ ] HTTP status codes remain unchanged.
398. [ ] Existing normal error behavior remains intact.
399. [ ] Tests cover every required special-character type across account ID, asset code, and policy name messages.
400. [ ] Final implementation is reviewed, tested, and ready for pull request.
