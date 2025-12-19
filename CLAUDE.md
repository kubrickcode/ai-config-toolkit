# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AI Config Toolkit - 다른 프로젝트들에서 사용할 Claude Code 설정들의 중앙 저장소.

## Repository Structure

```
.claude/                    # 영문 버전 (프로젝트에 복사하여 사용)
├── agents/                 # 전문 에이전트 정의
├── commands/               # 슬래시 커맨드
├── framework-rules/        # 프레임워크별 규칙 (React, Next.js, NestJS)
├── output-styles/          # 응답 스타일
└── rules/                  # 코딩 규칙
    ├── core/               # 공통 원칙
    ├── api/                # REST/GraphQL
    ├── languages/          # TypeScript, Go
    └── testing/            # 테스트

.claude.ko/                 # 한국어 버전 (동일 구조)
```

## Commands

```bash
just deps        # pnpm install
just lint        # 전체 포맷팅
just lint config # JSON/YAML/MD 포맷팅
```
